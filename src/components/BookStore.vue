<template>
	<div class="book-list">
    	<div class="list" v-for="book in myBook">
    		<div class="title">
    			<input type="number" v-model="selectedBook[$index]" min="0" value="0"> 
    			{{ book.title }} 
    		</div>
    	</div>
    	
    	
	</div>
	
	<div class="summary"><strong>คุณเลือกหนังสือ</strong> {{ totalSelectedBook }} เล่ม</div>
	<div class="summary"><strong>ราคารวม</strong> {{ totalPrice }} บาท</div>
	<div class="summary"><strong>ส่วนลด</strong> {{ discount }} บาท</div>
	<div class="summary"><strong>รวมสุทธิ</strong> {{ totalPrice - discount || '0' }} บาท</div>
</template>

<script>
	import BookList from '../data/Book'
	export default {
		data () {
			return {
				selectedBook: [],
				totalSelectedBook: 0,
				discount: 0,
				discountP: 0,
				totalPrice: 0,
				myBook: BookList	
			}
		},
		ready: function() {
			console.log('!! Naidin Shop Ready !!');
			
		},
		watch: {
			'selectedBook': function(val, oldVal) {
				
				//Get total selected book
				this.totalSelectedBook = val.reduce((previous, current, index, array)=>{
					return previous + parseInt(current)
				},0)
			
				//Calculate Total Price
				this.totalPrice = this.totalSelectedBook*100
				
				//filter out zero book
				let uniqueBook = val.map((current, index, array)=>{
					return parseInt(current)
				}).filter((current, index, array)=>{
					return current !== 0
				})
				
				// Sort Array (min to max)
				uniqueBook.sort((a,b)=>{
					return a - b
				})

				//Calculate Discount
				this.discount = 0
				let dupCount  = 0
				while( uniqueBook.length > 1 ){
					dupCount = uniqueBook[0]
					this.discount += Math.floor(((dupCount*uniqueBook.length)*100) * ((uniqueBook.length-1)*0.1))
					
					uniqueBook = uniqueBook.map((current, index, array)=>{
						return current - uniqueBook[0]
					}).filter((current, index, array)=>{
						return current > 0
					})
				}
			}
		}
		
	}
</script>

<style scoped>
	.book-list {
		max-width: 600px;
	}
	.list {
		background-color: #d4d4d4;
		padding: 15px 20px;
		margin-bottom: 10px;
	}
	.price {
		margin-left: 20px;
	}
	.title {
		
	}
	.summary {
		text-align: left;
	}
</style>
