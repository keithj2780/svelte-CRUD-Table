<script>
    import { createEventDispatcher, onMount } from 'svelte';
    import {icontrash, iconedit, iconcreate, iconsave} from './svgIcon'

    export let config;
    export let data;

    let rowIdxInEditMode = -1;

    let dispatch = createEventDispatcher();

    onMount(()=>{
        console.log(data);
    });
    function addRow(e) {
        data=[...data,{}]; 
        rowIdxInEditMode=data.length-1;
        dispatch('addrow',rowIdxInEditMode);
    }
    //  column resizer
    let resizeMouseDownX=-1
    let origWidth = 0;

    function resizemousedown(e,i) {
        resizeMouseDownX=e.clientX;
        origWidth = config.columns_setting[i].width;
        e.target.setPointerCapture(e.pointerId);
    }
    function resizemousemove(e,i) {
        if (resizeMouseDownX == -1) return;
        let delta = e.clientX-resizeMouseDownX;
        config.columns_setting[i].width = Math.max(5,origWidth+delta);        //  min of at least 5 pixels wide
        //console.log(origWidth,delta,i,resizeMouseDownX);
    }
    function resizemouseup(e,i) {
        resizeMouseDownX = -1;
        origWidth=0;
    }
</script>

<div class="container">
    <table>
        <thead class="colHeader">
            {#each config.columns_setting as col,colIdx}
                {#if col.show}
                    <td style={'width:'+col.width+'px'}>
                        <div  class="resizableHeader">
                            <div class="colDesc">{col.description}</div>
                            {#if col.resizable}
                                <div class='resizer'
                                    on:pointerdown={(e)=>resizemousedown(e,colIdx)}
                                    on:pointermove={(e)=>resizemousemove(e,colIdx)}
                                    on:pointerup={(e)=>resizemouseup(e,colIdx)}
                                    />
                            {/if}
                        </div>
                    </td>
                {/if}
            {/each}
            {#if config.options.includes('CREATE')}
                <td class="td">
                    <div class="btnHoverGreen" on:click={(e)=>addRow(e)}>
                        {@html iconcreate}
                    </div>
                </td>
            {/if}
     </thead>

        {#each data as row, rowIdx}
            {#if rowIdx >= config.row_settings.firstRow && rowIdx < config.row_settings.lastRow}
                <tr class={rowIdx%2?'trOdd':'trEven'} 
                    on:click={()=>{if (rowIdxInEditMode!=rowIdx) rowIdxInEditMode=-1;}}
                    on:dblclick={()=>{if (config.options.includes('DBLCLICKEDIT')) rowIdxInEditMode=rowIdx;}}
                    >
                    {#each config.columns_setting as col}
                        {#if col.show}
                            <td class="td">
                                <!-- If we're editing this row -->
                                {#if rowIdx == rowIdxInEditMode && col.edit}
                                    {#if col.type == 'select'}
                                        <select bind:value={row[col.name]}>
                                            {#each col.options as opt}
                                                <option value={opt.value}>{opt.text}</option>
                                            {/each}
                                        </select>
                                    {:else if col.type == 'date'}
                                        <input bind:value={row[col.name]} type="date" style={'width:'+col.width+'px;'}/>
                                    {:else if col.type == 'color'}
                                        <input bind:value={row[col.name]} type="color" style={'width:'+col.width+'px;'}/>
                                    {:else if col.type == 'email'}
                                        <input bind:value={row[col.name]} type="email" style={'width:'+col.width+'px;'}/>
                                    {:else if col.type == 'text' || col.type == 'html'  || col.type == undefined }
                                        <textarea bind:value={row[col.name]} rows={4} style={'width:'+col.width+'px;'}></textarea>
                                    {/if}
                                <!-- Just display the data -->
                                {:else if row[col.name] != undefined}
                                    {#if col.type=='html'}
                                        <!-- usual warnings about displaying raw HTML -->
                                        {@html row[col.name]}
                                    {:else if col.type=='select'}
                                        <!-- this next check is only required because a data value may have an unknown option value -->
                                        {#if col.options.find(option=>option.value==row[col.name])}
                                            {col.options.find(option=>option.value==row[col.name]).text}
                                        {:else}
                                            No option for {row[col.name]}
                                        {/if}
                                    {:else if col.type=='color'}
                                        <input value={row[col.name]} type="color" disabled/>
                                    {:else if col.type=='date'}
                                        {row[col.name]}
                                    {:else}
                                        {row[col.name]}
                                    {/if}
                                {/if}
                            </td>
                        {/if}
                    {/each}
                    <td class="td">
                        {#if rowIdx == rowIdxInEditMode}
                            <div class="btnHoverGreen" on:click={()=>{rowIdxInEditMode=-1;dispatch('endedit',rowIdx);}}>
                                {@html iconsave}
                            </div>
                        {:else}
                            {#if config.options.includes('DELETE')}
                                <div class="btnHoverRed" on:click={(e)=>dispatch('requestdeleterow',rowIdx)}>
                                    {@html icontrash}
                                </div>
                            {/if}
                            {#if config.options.includes('EDIT')}
                                <div class="btnHoverGreen" on:click|stopPropagation={(e)=>{rowIdxInEditMode=rowIdx;dispatch('startedit',rowIdx);}}>
                                    {@html iconedit}
                                </div>
                            {/if}
                        {/if}
                    </td>
                </tr>
            {/if}
        {/each}
    </table>
</div>

<style>
    table {
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", sans-serif;
    }
    textarea {
        resize: vertical;
        min-height: 1.3em;
        padding: 1px 1px;
        font-size: 1.05em;
        font-weight: 300;
        border-bottom: 0.5px solid #5f5f5f;
    }
    .colHeader {
        font-size: 20px;
        text-align: center;
    }
    .trOdd {
        background-color: rgb(228, 228, 228);
    }
    .trEven {
        background-color: rgb(216, 216, 216);
    }
    .trOdd:hover , .trEven:hover {
        background-color: rgb(206, 206, 206);
    }
    .btnHoverRed:hover {
        fill: rgb(255, 53, 46);
    }
    .btnHoverGreen:hover {
        fill: rgb(147, 255, 162);
    }
    .resizableHeader {
        display:flex;
        justify-content: space-between;
    }
    .colDesc {
        height:100%;
    }
    .resizer {
        width:5px;
    }
    .resizer:hover {
        cursor:ew-resize;        
    }
</style>