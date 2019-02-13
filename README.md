---


---

<h1 id="mvvm-guidelines">MVVM Guidelines</h1>
<ul>
<li><a href="#inputs---outputs">Inputs - Outputs</a>
<ul>
<li><a href="#principles">Principles</a></li>
<li><a href="#how-to">How-to</a></li>
<li><a href="#why-inputs--outputs">Why Inputs - Outputs?</a></li>
<li><a href="#example">Example</a></li>
<li><a href="#reference">Reference</a></li>
</ul>
</li>
</ul>
<h2 id="inputs---outputs">Inputs - Outputs</h2>
<h4 id="principles">Principles</h4>
<p>inputs is a set of actions and events that have impacts on viewModel such as the tap action on a button, or the viewDidLoad event.<br>
outputs represents changes that views should reflect.<br>
Since ouputs may change over time, it’s best to return an Observable (in RxSwift context) for each ouput.<br>
Behaviors defined in inputs should not be expressed as Variable because we don’t need the inputs to be obseravable.</p>
<h4 id="how-to">How to</h4>
<pre class=" language-swift"><code class="prism  language-swift">protocol <span class="token builtin">LoginViewModelInputs</span> <span class="token punctuation">{</span>
	<span class="token keyword">var</span> viewDidLoad<span class="token punctuation">:</span> <span class="token builtin">PublishRelay</span><span class="token operator">&lt;</span><span class="token builtin">Void</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
	<span class="token keyword">var</span> tapSubmit<span class="token punctuation">:</span> <span class="token builtin">PublishRelay</span><span class="token operator">&lt;</span><span class="token builtin">Void</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>

protocol <span class="token builtin">LoginViewModelOutputs</span> <span class="token punctuation">{</span>
	<span class="token keyword">var</span> validInput<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">Bool</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
	<span class="token keyword">var</span> isLoading<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">Bool</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>

protocol <span class="token builtin">LoginViewModelType</span> <span class="token punctuation">{</span>
	<span class="token keyword">var</span> inputs<span class="token punctuation">:</span> <span class="token builtin">LoginViewModelInputs</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
	<span class="token keyword">var</span> ouputs<span class="token punctuation">:</span> <span class="token builtin">LoginViewModelOutputs</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<p>This is what LoginViewModel looks like:</p>
<pre class=" language-swift"><code class="prism  language-swift"><span class="token keyword">final</span> <span class="token keyword">class</span> <span class="token class-name">LoginViewModel</span><span class="token punctuation">:</span> <span class="token builtin">LoginViewModelType</span><span class="token punctuation">,</span> <span class="token builtin">LoginViewModelInputs</span><span class="token punctuation">,</span> <span class="token builtin">LoginViewModelOutputs</span> <span class="token punctuation">{</span>
	<span class="token keyword">var</span> inputs<span class="token punctuation">:</span> <span class="token builtin">LoginViewModelInputs</span> <span class="token punctuation">{</span> <span class="token keyword">return</span> <span class="token keyword">self</span> <span class="token punctuation">}</span>
	<span class="token keyword">var</span> ouputs<span class="token punctuation">:</span> <span class="token builtin">LoginViewModelOutputs</span> <span class="token punctuation">{</span> <span class="token keyword">return</span> <span class="token keyword">self</span> <span class="token punctuation">}</span>

	<span class="token comment">// MARK: - Inputs</span>
	<span class="token keyword">var</span> viewDidLoad<span class="token punctuation">:</span> <span class="token builtin">PublishRelay</span><span class="token operator">&lt;</span><span class="token builtin">Void</span><span class="token operator">&gt;</span>
	<span class="token keyword">var</span> tapSubmit<span class="token punctuation">:</span> <span class="token builtin">PublishRelay</span><span class="token operator">&lt;</span><span class="token builtin">Void</span><span class="token operator">&gt;</span>
	
	<span class="token comment">// MARK: - Ouputs</span>
	<span class="token keyword">var</span> validInput<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">Bool</span><span class="token operator">&gt;</span>
	<span class="token keyword">var</span> isLoading<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">Bool</span><span class="token operator">&gt;</span>

	<span class="token keyword">init</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<h4 id="why-inputs--outputs">Why Inputs / Outputs?</h4>
<p>First of all, by using protocols like this, we achieve higher level of abstraction. Therefore, our code is more behavior-oriented and easier to test.</p>
<p>Another advantage of this protocol-based convention is readability in unit tests.<br>
By looking at the codes related to inputs calls, we quickly have a sense of the scenarios we are trying to simulate. Similarly, what we expect to see are reflected upon outputs.</p>
<h2 id="naming">Naming</h2>
<p>Descriptive and consistent naming makes software easier to read and understand.</p>
<h4 id="inputs-use-events-name-instead-of-command-sentences">Inputs use events name instead of command sentences</h4>
<p><strong>Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift">protocol <span class="token builtin">LoginViewModelInputs</span> <span class="token punctuation">{</span>
	<span class="token keyword">var</span> viewDidLoad<span class="token punctuation">:</span> <span class="token builtin">PublishRelay</span><span class="token operator">&lt;</span><span class="token builtin">Void</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
	<span class="token keyword">var</span> tapSubmit<span class="token punctuation">:</span> <span class="token builtin">PublishRelay</span><span class="token operator">&lt;</span><span class="token builtin">Void</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<p><strong>Not Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift">	<span class="token keyword">var</span> fetchItems<span class="token punctuation">:</span> <span class="token builtin">PublishRelay</span><span class="token operator">&lt;</span><span class="token builtin">Void</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
	<span class="token keyword">var</span> submitItem<span class="token punctuation">:</span> <span class="token builtin">PublishRelay</span><span class="token operator">&lt;</span><span class="token builtin">Void</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
</code></pre>
<h4 id="outputs-use-status-that-represent-data-for-instead-of-what-is--data-meaning">Outputs use status that represent data for instead of what is  data meaning</h4>
<p><strong>Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift">protocol <span class="token builtin">LoginViewModelOutputs</span> <span class="token punctuation">{</span>
	<span class="token keyword">var</span> isHideLeftMenu<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">Bool</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<p><strong>Not Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift">	<span class="token keyword">var</span> isGuest<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">Bool</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
</code></pre>
<h4 id="after-bindingsubscribe-alway-add-dispose-to-dispose-bag">After binding/subscribe alway add dispose to dispose bag</h4>
<p><strong>Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift"><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
<span class="token punctuation">.</span><span class="token function">bind</span><span class="token punctuation">(</span>to<span class="token punctuation">:</span> viewModel<span class="token punctuation">.</span>input<span class="token punctuation">.</span>didBecomeActive<span class="token punctuation">)</span>
<span class="token punctuation">.</span><span class="token function">disposed</span><span class="token punctuation">(</span>by<span class="token punctuation">:</span> bag<span class="token punctuation">)</span>
</code></pre>
<p><strong>Not Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift"><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span>
<span class="token punctuation">.</span><span class="token function">bind</span><span class="token punctuation">(</span>to<span class="token punctuation">:</span> viewModel<span class="token punctuation">.</span>input<span class="token punctuation">.</span>didBecomeActive<span class="token punctuation">)</span>
</code></pre>
<h4 id="binding-method">Binding method</h4>
<p><strong>Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift"><span class="token keyword">private</span> <span class="token keyword">func</span> <span class="token function">bindInputs</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token keyword">private</span> <span class="token keyword">func</span> <span class="token function">bindOutputs</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
</code></pre>
<p><strong>Not Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift"><span class="token keyword">private</span> <span class="token keyword">func</span> <span class="token function">configure</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
</code></pre>
<h4 id="use-dependency-injection-for-use-cases-to-view-models">Use Dependency Injection for use-cases to view models</h4>
<p><strong>Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift"><span class="token keyword">init</span><span class="token punctuation">(</span>provider<span class="token punctuation">:</span> <span class="token builtin">Domain</span><span class="token punctuation">.</span><span class="token builtin">UseCaseProvider</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">let</span> useCase <span class="token operator">=</span> provider<span class="token punctuation">.</span><span class="token function">makeHomeScreenUseCase</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>
	<span class="token constant">OR</span>
<span class="token keyword">init</span><span class="token punctuation">(</span>provider<span class="token punctuation">:</span> <span class="token builtin">Domain</span><span class="token punctuation">.</span><span class="token builtin">UseCaseProvider</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">let</span> useCase <span class="token operator">=</span> provider<span class="token punctuation">.</span><span class="token function">makeHomeScreenUseCase</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
	<span class="token keyword">let</span> network <span class="token operator">=</span> provider<span class="token punctuation">.</span>network
<span class="token punctuation">}</span>
	<span class="token constant">PS</span><span class="token punctuation">.</span> <span class="token builtin">Depends</span> on <span class="token builtin">Project</span> and <span class="token builtin">Team</span>
</code></pre>
<p><strong>Not Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift"><span class="token keyword">init</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	<span class="token keyword">let</span> useCase <span class="token operator">=</span> <span class="token function">UseCase</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>

<span class="token keyword">init</span><span class="token punctuation">(</span>network<span class="token punctuation">:</span> <span class="token builtin">Network</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
<span class="token punctuation">}</span>
</code></pre>
<h4 id="use-enum-to-represent-image-named--color">Use Enum to represent Image named / color</h4>
<p><strong>Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift">
<span class="token keyword">enum</span> <span class="token builtin">LeftImage</span><span class="token punctuation">:</span> <span class="token builtin">String</span> <span class="token punctuation">{</span>
	<span class="token keyword">case</span> discount <span class="token operator">=</span> <span class="token string">"image1"</span>
	<span class="token keyword">case</span> <span class="token constant">BOGO</span> <span class="token operator">=</span> <span class="token string">"image2"</span>
<span class="token punctuation">}</span>

protocol <span class="token builtin">LoginViewModelOutputs</span> <span class="token punctuation">{</span>
	<span class="token keyword">var</span> leftImage<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">LeftImage</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<p><strong>Not Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift">protocol <span class="token builtin">LoginViewModelOutputs</span> <span class="token punctuation">{</span>
	<span class="token keyword">var</span> leftImage<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">String</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
	<span class="token keyword">var</span> headerBarColor<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">BarColor</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<h4 id="not-provine-the-hold-model--only-necessary-data">NOT provine the hold model / ONLY necessary data</h4>
<p><strong>Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift">protocol <span class="token builtin">LoginViewModelOutputs</span> <span class="token punctuation">{</span>
	<span class="token keyword">var</span> fullName<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">String</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<p><strong>Not Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift">protocol <span class="token builtin">LoginViewModelOutputs</span> <span class="token punctuation">{</span>
	<span class="token keyword">var</span> person<span class="token punctuation">:</span> <span class="token builtin">Driver</span><span class="token operator">&lt;</span><span class="token builtin">Person</span><span class="token operator">&gt;</span> <span class="token punctuation">{</span> <span class="token keyword">get</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<h4 id="present-screens-with-coordinator">Present screens with coordinator</h4>
<p><strong>Preferred:</strong></p>
<pre class=" language-swift"><code class="prism  language-swift"><span class="token keyword">init</span><span class="token punctuation">(</span>coordinator<span class="token punctuation">:</span> <span class="token builtin">Coordinable</span><span class="token punctuation">,</span> useCaseProvider<span class="token punctuation">:</span> <span class="token builtin">Domain</span><span class="token punctuation">.</span><span class="token builtin">UseCaseProvider</span><span class="token punctuation">)</span><span class="token punctuation">{</span>
	<span class="token keyword">let</span> viewModel <span class="token operator">=</span> <span class="token function">SignUpViewModel</span><span class="token punctuation">(</span>coordinator<span class="token punctuation">:</span> coordinator<span class="token punctuation">)</span>
	<span class="token keyword">let</span> scene<span class="token punctuation">:</span> <span class="token builtin">OnboardScene</span> <span class="token operator">=</span> <span class="token punctuation">.</span><span class="token function">signUp</span><span class="token punctuation">(</span>viewModel<span class="token punctuation">)</span>
	coordinator<span class="token punctuation">.</span><span class="token function">transition</span><span class="token punctuation">(</span><span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span>scene<span class="token punctuation">:</span> scene<span class="token punctuation">,</span> animated<span class="token punctuation">:</span> <span class="token boolean">true</span><span class="token punctuation">)</span><span class="token punctuation">)</span>

</code></pre>
<h4 id="example-code">Example Code</h4>
<p><a href="https://bitbucket.org/appsynth/day1/src/master/">https://bitbucket.org/appsynth/day1/src/master/</a></p>
<h4 id="clean-architecture-with--rxswift">Clean architecture with  <a href="https://github.com/ReactiveX/RxSwift">RxSwift</a></h4>
<p><a href="https://github.com/sergdort/CleanArchitectureRxSwift">https://github.com/sergdort/CleanArchitectureRxSwift</a></p>
<h4 id="reference">Reference</h4>
<p>[1] <a href="https://github.com/kickstarter/native-docs/blob/master/vm-structure.md">https://github.com/kickstarter/native-docs/blob/master/vm-structure.md</a><br>
[2] <a href="https://github.com/kickstarter/native-docs/blob/master/inputs-outputs.md">https://github.com/kickstarter/native-docs/blob/master/inputs-outputs.md</a></p>

