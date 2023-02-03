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
		createdContact = [...createdContact, {id: Math.random(), name: name, jobTitle: title, imageUrl: image, desc: description}];
		formState = 'done';
  }

  //slice = 자바스크립트 내장 객체, 새 배열을 반환하고 숫자 /두번째 위치 index/ 위치에서 시작. 
  /**
   스벨트 문제점. 마지막 요소를 삭제하지만 목록을 살펴보고 어떤 요소를 변경해야 하는지 데이터 구조를 반영하기 위해 DOM에서 어디를 변경해야 하는지 확인하는데 배열의 유일한 요소인 userName이라는 것을 확인 하고 랜더링된 요소 하나만 업데이트 한다.
   문제는 잘못된 요소를 제거 했기 때문에 다시 구축되거나 실행되지 않고 해당 데이터가 /initialName가 잘못된 요소에 연결되었던 것.

  개선 방법. 스벨트가 DOM 마크업에 연결하는데 도움이 되는 고유 식별자가 있어야 한다. ID부여
   */
  
  function deleteFirst(){
    createdContact = createdContact.slice(1);
  }
  //slice = 첫 번째 요소에서 시작, -1/마지막 요소 전/까지 전체 배열 제공
  function deleteLast(){
    createdContact = createdContact.slice(0 , -1)
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
<button on:click={deleteFirst}>Delete First</button>
<button on:click={deleteLast}>Delete Last</button>
<!-- formState가 done일 때 명함 데이터 출력 되도록 입력 유효성 검사-->
{#if formState === 'invalid'}
	<h1>다시 작성하세요</h1>
{:else}
	<p>입력 하시고 버튼 클릭 하세요</p>
{/if}

{#each createdContact as contact, index (contact.id)}
<h2># {index + 1}</h2>
<ContactCard userName={contact.name} jobTitle={contact.jobTitle} description={contact.desc} userImage={contact.image} />
{:else}
	<p>연락처가 없습니다. 연락처를 추가하세요</p>
{/each}
