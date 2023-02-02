<script>
  import ContactCard from "./ContactCard.svelte";

  let name = "Max";
  let title = "";
  let image = "";
  let description = "";
  let formState = 'empty'

  let createdContact = [];

  //name.trim()은 시작과 끝에 있는 초과 공백 제거하고 length가 0인지 확인
  //버튼을 누를때 유효한 입력을 넣었는지 여부에 따라 formState를 수동으로 설정
  //formState가 done일 때 명함 데이터 출력 되도록 설정, 충족되지 않으면 return문으로 인해 다시 감
  function addContact(){
	if(name.trim().length == 0 || 
		title.trim().length == 0 ||
		image.trim().length == 0 ||
		description.trim().length == 0)
		{
			formState = 'invalid';
			return;
		}
		createdContact = [...createdContact, {name: name, jobTitle: title, imageUrl: image, desc: description}];
		formState = 'done';
  }
</script>

<style>
  #form {
    width: 30rem;
    max-width: 100%;
  }
</style>

<div id="form">
  <div class="form-control">
    <label for="userName">User Name</label>
    <input type="text" bind:value={name} id="userName" />
  </div>
  <div class="form-control">
    <label for="jobTitle">Job Title</label>
    <input type="text" bind:value={title} id="jobTitle" />
  </div>
  <div class="form-control">
    <label for="image">Image URL</label>
    <input type="text" bind:value={image} id="image" />
  </div>
  <div class="form-control">
    <label for="desc">Description</label>
    <textarea rows="3" bind:value={description} id="desc" />
  </div>
</div>

<button on:click={addContact}>Add Contact Card</button>
<!-- formState가 done일 때 명함 데이터 출력 되도록 입력 유효성 검사-->
{#if formState === 'invalid'}
	<h1>다시 작성하세요</h1>
{:else}
	<p>입력 하시고 버튼 클릭 하세요</p>
{/if}

{#each createdContact as contact, index}
<h2># {index + 1}</h2>
<ContactCard userName={contact.name} jobTitle={contact.jobTitle} description={contact.desc} userImage={contact.image} />
{:else}
	<p>연락처가 없습니다. 연락처를 추가하세요</p>
{/each}
