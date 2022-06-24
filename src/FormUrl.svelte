<script>
    import {createEventDispatcher} from 'svelte';
    const dispatch = createEventDispatcher(); 
    

    let link = 'brur.ac.bd';
    
    const submitHandler = async() => {
        let altTextImg = [];
         let noAltTextImg = [];
         let garbage = [];
       
         dispatch('results', {isLoading: true});
	   
       try{
        const response = await fetch('https://api.codetabs.com/v1/proxy?quest='+ link)
        const sourceCode = await response.text();
        
        let sourceArray = sourceCode.split('\n');
      
        for(let i in sourceArray){
            
            if(sourceArray[i].includes('<img')){
                let g = sourceArray[i];
                
                let img = '<img' + sourceArray[i].split('<img')[1];
                    img = img.split('>')[0]+'>';
             if((img.includes('alt')) &&  (img.includes('src') || img.includes('data-src') || img.includes('data-srcset'))){
                img = img.replace('data-src', 'src')
                img = img.replace('data-srcset', 'src')
                img = img.replace('srcset', 'src')

                img = img.split('src="')[1]
                img = img.split('"')[0]
                if(img.includes('./')){
                    img = img.split('./')[1]
                    img = 'https://www.'+link+'/'+img;
                }
                altTextImg.push(img)
             } else if(img.includes('src') || img.includes('data-src') || img.includes('data-srcset')){
                img = img.replace('data-src', 'src')
                img = img.replace('data-srcset', 'src')
                img = img.replace('srcset', 'src')
                img = img.replace('width', 'xwidth')
                img = img.replace('height', 'xheight')
                img = img.split('src="')[1]
                img = img.split('"')[0]
                if(img.includes('./')){
                    img = img.split('./')[1]
                    img = 'https://www.'+link+'/'+img;
                }
                noAltTextImg.push(img)
             } else{
                garbage.push(g);
             }
            }

        }
        dispatch('results', {altTextImg, noAltTextImg, garbage, isLoading: false});
       }
       catch(err){
        console.log(err)
       }
    

      
       
        

	}
	
</script>

<form on:submit|preventDefault={submitHandler}>
    <input type="text" bind:value={link} placeholder="Put url here..."/>
    <button>Look up</button>
</form>

<style>

</style>