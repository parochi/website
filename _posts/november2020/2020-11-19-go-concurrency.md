---
layout: post
title: Go Concurrency - A typical production use
date: 2020-11-19
related_image: /assets/images/2020-11-19.png
---

<div class="view overlay">
	<img class="card-img-top" src="{{ page.related_image }}" alt="Card image cap">
    <a href="#!">
        <div class="mask rgba-white-slight"></div>
    </a>
</div>

Go handles concurrency in a slightly different way than the other programming languages. The effective Go has a slogan around their new concept 
 
"Do not communicate by sharing memory; instead, share memory by communicating".
 
These Go routines are unique and they are not operating system threads. It is a function executing concurrently with other goroutines in the same address space. It is lightweight, costing little more than the allocation of stack space. More you can read at the official documentation site  [here](https://golang.org/doc/effective_go.html#concurrency) 

Enough of the concepts, let us delve into the code and apply these concepts. First, we will implement some example code to understand the concepts, after that we will do an identical code that we typically use in production. A typical goroutine is written below.

<pre><code class="go">
sayHello := func() {
    fmt.Println("Techiebar is awesome!")
}
go sayHello()
// continue doing rest of your code
</code></pre>

Now, when you execute the above snippet, you might not see the output string `Techiebar is awesome!` that is due to the fact that the main thread might get finished its job well before the goroutine. One of the ways to handle this issue is by using Go `sync` package, check the below snippet.

<pre><code class="go">
var wg sync.WaitGroup

wg.Add(1)                       
go func() {
    defer wg.Done()             
    fmt.Println("7:00AM Techiebar is closed!")
}()

wg.Add(1)                       
go func() {
    defer wg.Done()             
    fmt.Println("5:00PM Techiebar is Opened!")
}()

wg.Wait()                       
fmt.Println("All goroutines completed.")
</code></pre>
 
`WaitGroup` is a great way to wait for a set of concurrent operations to complete when you either don’t care about the result of the concurrent operation or you have other plans to collect the results.
 
There is another interesting method in the sync package is `once.Do(<calling method>)`, This will make sure that the calling method will be called once, even though it triggered by different routines. Try that by writing an example and check the behavior. 
 
Now, the interesting concept is about co-ordinating the multiple Goroutines using channels.
Yes, Channel is a great way of sending and receiving the values. A typical channel example looks like below.

<pre><code class="go">
func main() {
  //We have some integers
  elems := []int{1, 2, 3, 4, 5, 0} 

  //Handle the sum of all integers

  sum := func(s []int, c chan int) {
    sum := 0
    for _, v := range s {
      sum += v
    }
    c &lt;- sum // sending sum to the channel
  }
  //We have created a channel of type int
  //Do note that, the channel can hold only one integer value
  // at a time
  receiver := make(chan int)
  go sum(elems, receiver)
  total := &lt;-receiver // receiver returns the result of the sum

  fmt.Println(total)
}
</code></pre>
 
So far everything is fine, we have routines, waitGroups, channels. But, what does a typical production code looks like to use channels with multiple goroutines. Now, let us take an example scenario where we have some 1000 requests[In production we will face 1000's requests] that need to be processed and each request is independent and can be processed with a separate goroutine. That means if we spawn 10 goroutines as a worker pool to process the 1000 requests. Let us see how the code looks like.

<pre><code class="go">
//FullName is first + last name
type FullName struct {
	FirstName string
	LastName  string
}

func main() {
	fnChannel := make(chan FullName)
	totalWorkers := 10
	var wg sync.WaitGroup

	// You have created 10 routines to handle your 1000 requests
	for i := 0; i &lt; totalWorkers; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			for fn := range fnChannel {
				printFullName(fn)
			}
		}()
	}

	// You have 1000 requests, needs to be processed
	for j := 0; j &lt;= 1000; j++ {
		// You are pushing one by one to the channel
		fnChannel &lt;- FullName{
			FirstName: fmt.Sprintf("F%d", j),
			LastName:  fmt.Sprintf("L%d", j),
		}
	}
	close(fnChannel)
	wg.Wait()
	fmt.Println("Everything completed")

}

func printFullName(fn FullName) {
	fmt.Printf("%s %s\n", fn.FirstName, fn.LastName)
}
</code></pre>

 
In the above example code, at line number 8 we have created a channel that accepts the `FullName` data structure and we have created 10 workers to take up our task of processing the full name. Finally, at line number 26 we started pushing the data to the channel. Now all the 10 goroutines start processing the given data and accept another set of data until the channel is closed.
 
For more information, it is always recommended to go through the Effective  [Go concurrency](https://golang.org/doc/effective_go.html#concurrency)  documentation.
 
Share your constructive comments or suggestions to improve the content. 

<div id="disqus_thread"></div>
<script>
   /*
    *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
    *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    
    */
    var disqus_config = function () {
    this.page.url = "https://www.parochi.xyz/2020/11/19/go-concurrency.html";  // Replace PAGE_URL with your page's canonical URL variable
    this.page.identifier = "20201119"; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    
    (function() { // DON'T EDIT BELOW THIS LINE
    var d = document, s = d.createElement('script');
    s.src = 'https://parochi-xyz.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>