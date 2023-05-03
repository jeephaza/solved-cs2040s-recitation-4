Download Link: https://assignmentchef.com/product/solved-cs2040s-recitation-4
<br>
<h1>Problem 1.        B-Trees</h1>

Image credit: <a href="http://people.cs.georgetown.edu/~jfineman/">Jeremy Fineman</a>

In the first lecture you were asked whether the fastest way to search for data is to store them in an array, sort them and then perform binary search. The answer to that question is — surprisingly — no! You have seen Binary Search Trees (BSTs), where we design a DS that always maintains data in an (hierarchically) ordered manner. This will allow us to avoid the overhead of sorting before we search. However, you also learnt that unbalanced BSTs can be terribly inefficient for insertion, deletion and search operations (<em>why?</em>).

In this week’s lecture, you were introduced to the idea of self-balancing BST such as an AVL-tree (other variants exist!). Today, we will learn about <strong>B-trees</strong>, which is another (very important!) self-balancing tree DS for maintaining ordered data. It is important to note that the ‘B’ in B-tree <em>does not </em>stand for “Binary” so don’t get confused between B-Trees and BSTs. They are not the same!

<h1>1         (<em>a,b</em>)-trees</h1>

Before we talk about B-trees, we first introduce its family (generalized form) which are called (<em>a,b</em>)trees. In an (<em>a,b</em>)-tree, <em>a </em>and <em>b </em>are parameters where 2 ≤ <em>a </em>≤ (<em>b</em>+1)<em>/</em>2. Respectively, <em>a </em>and <em>b </em>refer to the minimum and the maximum number of children an internal node in the tree (i.e. non-root, non-leaf) can have.

The main differences between binary trees and (<em>a,b</em>)-trees can be summarized in the following table.

<table width="511">

 <tbody>

  <tr>

   <td width="230"><strong>Binary trees</strong></td>

   <td width="281">(<em>a,b</em>)<strong>-trees</strong></td>

  </tr>

  <tr>

   <td width="230">Each node <em>has at most </em>2 children</td>

   <td width="281">Each node <em>can have more than </em>2 children</td>

  </tr>

  <tr>

   <td width="230">Each node <em>stores exactly one </em>key</td>

   <td width="281">Each node <em>can store multiple </em>keys</td>

  </tr>

 </tbody>

</table>

<h2>1.1        Rules</h2>

<em>Pro-Tip: </em>Review these rules by concurrently referring to the (2<em>,</em>4)-tree example in Figure 1.

<h3><strong>Rule 1 </strong>“(a,b)-child Policy”</h3>

<table width="284">

 <tbody>

  <tr>

   <td width="93"><strong>Node type</strong></td>

   <td width="49"><strong>#Keys</strong>Min</td>

   <td width="48">Max</td>

   <td width="42">Min</td>

   <td width="52"><strong>#Children</strong>Max</td>

  </tr>

  <tr>

   <td width="93"><strong>Root</strong></td>

   <td width="49">1</td>

   <td width="48"><em>b </em>− 1</td>

   <td width="42">2</td>

   <td width="52"><em>b</em></td>

  </tr>

  <tr>

   <td width="93"><strong>Internal</strong></td>

   <td width="49"><em>a </em>− 1</td>

   <td width="48"><em>b </em>− 1</td>

   <td width="42"><em>a</em></td>

   <td width="52"><em>b</em></td>

  </tr>

  <tr>

   <td width="93"><strong>Leaf</strong></td>

   <td width="49"><em>a </em>− 1</td>

   <td width="48"><em>b </em>− 1</td>

   <td width="42">0</td>

   <td width="52">0</td>

  </tr>

 </tbody>

</table>

<strong>Table 1: </strong>For each type of node, we can either define this rule in terms of the number of children or equivalently in terms of the number of keys permitted.

<em>With the exception of leaves, realize that the number of children is always one more than the number of keys (see Rule </em><em>2).</em>

<h3><strong>Rule 2 </strong>“Key-ordering”</h3>

<em>A </em><em>non-leaf node (i.e. root or internal) must have one more child than its number of keys. This is to ensure that all value ranges due to its keys are covered in its subtrees. We shall henceforth refer to the permitted range of keys within a subtree to be its </em><em>key range.</em>

<em>Specifically, for a non-leaf node with k keys and k </em>+ 1 <em>children:</em>

<ul>

 <li><em>Its keys in sorted order are v</em><sub>1</sub><em>,v</em><sub>2</sub><em>,…,v<sub>k</sub></em></li>

 <li><em>The subtrees due to its keys are t</em><sub>1</sub><em>,t</em><sub>2</sub><em>,…,t<sub>k</sub></em><sub>+1</sub></li>

</ul>

<em>Then:</em>

<ul>

 <li><em>First child t</em><sub>1 </sub><em>has key range </em>≤ <em>v</em><sub>1</sub></li>

 <li><em>Final child t<sub>k</sub></em><sub>+1 </sub><em>has key range &gt; v<sub>k</sub></em></li>

 <li><em>All other children t<sub>i </sub>where i </em>∈ [2<em>,k</em>] <em>has key range </em>(<em>v<sub>i</sub></em><sub>−1</sub><em>,v<sub>i</sub></em>]</li>

</ul>

<h3><strong>Rule 3 </strong>“Leaf depth”</h3>

<em>All </em><em>leaf nodes must all be at the same depth (from root).</em>

<strong>Test yourself: </strong>Given these rules, is a BSTs just an (<em>a,b</em>)-tree where <em>a </em>= 1 and <em>b </em>= 2? Why or why not?

<strong>B-trees </strong>are simply (<em>B,</em>2<em>B</em>)-trees. That is to say, they are a subcategory of (<em>a,b</em>)-trees such that <em>a </em>= <em>B </em>and <em>b </em>= 2<em>B</em>. For instance, when <em>B </em>= 2, we have a (2<em>,</em>4)-tree. (<em>FYI: </em>this is sometimes referred to as a (2,3,4)-tree). An example of a (2<em>,</em>4)-tree is given below in Figure 1.

<strong>Figure 1: </strong>A (2<em>,</em>4)-tree storing 18 keys. The range of keys contained within a subtree is indicated by the label on the edge from its parent.

<strong>Problem 1.a.       </strong>Is an (<em>a,b</em>)-tree balanced? If it is, which rule(s) ensures that? If not, why?

<strong>Problem 1.b.      </strong>What is the minimum and maximum height of an (<em>a,b</em>)-tree with <em>n </em><strong>keys</strong>?

<strong>Problem 1.c. </strong>Write down the pseudocode for searching an (<em>a,b</em>)-tree. What data structure should we use for storing the keys and subtree links in a node?

<strong>Problem 1.d.             </strong>What is the cost of searching an (<em>a,b</em>)-tree with <em>n </em><strong>nodes</strong>?

<h2>1.2        split operation</h2>

By definition, the three rules (Section 1.1) must hold before and after any operation that (<em>a,b</em>)-trees support. To ensure this, we need to take care of any potential violations that can occur across all operations. It should be obvious that insertion and deletion poses risks of violating the rules since they alter membership.

We will first look at insertion. This operation clearly risk nodes growing beyond permissible bounds. To address this, we will sometimes need to split large nodes into smaller ones. Firstly, keys in the violating node <em>z </em>are divided into two parts using the median index in its keylist (think partition step in QuickSort). We shall refer to the key at the median index as the <em>split key</em>. This ensures that each part contains half of <em>z</em>’s keys along with their associated subtree links. The split key is then inserted into <em>z</em>’s parent. Specifically, split operation on a non-root node <em>z </em>with <em>b </em>keys is outlined in the following algorithm:

<ol>

 <li>Find the median key <em>v<sub>m </sub></em>where the number of keys in <em>z </em>that are ≤ <em>v<sub>m </sub></em>(i.e. LHS) is b(<em>b </em>− 1)<em>/</em>2c and the number of keys <em>&gt; v<sub>m </sub></em>(i.e. RHS) is d(<em>b </em>− 1)<em>/</em> Since by definition <em>b </em>≥ 2<em>a</em>, the sizes of each half can be respectively rewritten as ≥b(2<em>a </em>− 1)<em>/</em>2c = <em>a </em>and ≥d(2<em>a </em>− 1)<em>/</em>2e = <em>a </em>− 1. Therefore both LHS and RHS each have at least <em>a </em>− 1 keys, satisfying Rule 1.</li>

</ol>

…               …               <em>t<sub>m                                                      </sub></em>…               …

<ol start="2">

 <li>In <em>z</em>, separate all keys ≤ <em>v<sub>m </sub></em>as well as their associated subtree links to form a new adjacent node <em>y</em>.</li>

</ol>

…               …                       <em>t<sub>m                                                      </sub></em>…               …

<ol start="3">

 <li>Since <em>y </em>will be missing a final child corresponding to key range <em>&gt; y<sub>m</sub></em><sub>−1</sub>, assign it to be <em>t<sub>m</sub></em>, the subtree associated with <em>v<sub>m</sub></em>. Remove the the association between <em>t<sub>m </sub></em>and <em>v<sub>m</sub></em>.</li>

</ol>

…               …                       <em>t<sub>m                                                      </sub></em>…               …

<ol start="4">

 <li>Move key <em>v<sub>m </sub></em>from <em>z </em>to <em>p </em>(<em>z</em>’s parent). This newly inserted key should immediately precede the key in <em>p </em>which is associated to node <em>z</em>.</li>

</ol>

…               …                       <em>t<sub>m                                           </sub></em>…               …

<ol start="5">

 <li>Add <em>y </em>as a child of <em>p </em>by associating it with newly inserted <em>v<sub>m </sub></em>in <em>p</em>. We can do this because all keys in <em>y </em>are guaranteed to be ≤ <em>v<sub>m </sub></em>by virtue of the split.</li>

 <li>Return (<em>y,v<sub>m</sub>,z</em>).</li>

</ol>

<strong>Test yourself: </strong>Why do we have to give one key to the parent as ‘offering’? <em>Hint: </em>Recall Rule 1.

You should convince yourself that such a split operation on a <strong>non-root </strong>node <em>z </em>is safe (i.e. postconditions satisfy all rules) so long as it satisfies the following conditions:

<ol>

 <li><em>z </em>contains ≥ 2<em>a </em>keys, because after splitting

  <ul>

   <li>LHS has ≥ <em>a </em>− 1 keys</li>

   <li>RHS has ≥ <em>a </em>keys</li>

  </ul></li>

 <li><em>z</em>’s parent contains ≤ <em>b </em>− 2 keys</li>

</ol>

<strong>Test yourself: </strong>What happens if after the split operation, the parent node has ≥ <em>b </em>keys?

The algorithm outlined before applies to splitting a non-root node <em>z</em>, but what happens when <em>z </em>is actually the root node? What’s different now is that there is no parent to offer the split key <em>v<sub>m </sub></em>to! Hence following the same procedure, now we simply create a new node, offer it the split key <em>v<sub>m </sub></em>and then promote it to become the new root node. Recall from Rule 1 that we have made it legal to have a root with one key — the purpose of that is to simply support this root-splitting operation! Specifically, the root-splitting operation can be outlined as follows:

<table width="624">

 <tbody>

  <tr>

   <td width="114">Old steps</td>

   <td width="510">1.   Pick median key <em>v<sub>m </sub></em>as split key2.   Split <em>z </em>into LHS and RHS using <em>v<sub>m</sub></em>3.   Create a new node <em>y</em>4.   Move LHS split from <em>z </em>to <em>y</em></td>

  </tr>

  <tr>

   <td width="114">New steps</td>

   <td width="510">5.   Create a new empty node <em>r</em>6.   Insert <em>v<sub>m </sub></em>into <em>r</em>7.   Promote <em>r </em>to new root node8.   Assign <em>y </em>and <em>z </em>to be the left and right child of <em>r </em>respectively9.   Assign previous subtree <em>t<sub>m </sub></em>associated with <em>v<sub>m </sub></em>to be the final child of <em>y</em></td>

  </tr>

 </tbody>

</table>

<strong>Problem 1.e. </strong>Come up with an example each for splitting an internal node, and for splitting a root node.

<h2>1.3        Insertion</h2>

Alike BSTs, new items are also inserted only in the leaves of an (<em>a,b</em>)-tree. With the split operation defined, it is now easy to specify the full insertion procedure of an item <em>x </em>into the tree:

<ol>

 <li>Start at node <em>w </em>= <em>root</em></li>

 <li>Repeat until <em>w </em>is a leaf:

  <ul>

   <li>If <em>w </em>contains <em>b </em>− 1 keys,

    <ul>

     <li>Split <em>w </em>into <em>y </em>(LHS) and <em>z </em>(RHS) using split key <em>v<sub>m</sub></em></li>

     <li>Examine split key <em>v<sub>m </sub></em>returned by the split operation

      <ul>

       <li>If <em>x </em>≤ <em>v<sub>m</sub></em>, set <em>w </em>= <em>y</em></li>

       <li>Else, set <em>w </em>= <em>z</em></li>

      </ul></li>

     <li>Search for <em>x </em>in the keylist of <em>w</em>

      <ul>

       <li>If <em>x </em>is larger than all the keys, set <em>w </em>to be the final child</li>

       <li>Else, look for the first key <em>v<sub>i </sub></em>≥ <em>x </em>and set <em>w </em>to be <em>t<sub>i </sub></em>(subtree with key range ≤ <em>v<sub>i</sub></em>)</li>

      </ul></li>

    </ul></li>

  </ul></li>

</ol>

<ol start="3">

 <li>Add <em>x </em>to <em>w</em>’s keylist</li>

</ol>

Notice in the procedure outlined above, we preemptively split any nodes we encounter along the way which are at maximum key capacity (i.e. <em>b</em>−1). This is known as a <em>proactive </em>approach. With such an approach, we are guaranteed that the parent of the node to be split will not have too many keys after the addition of the split key. As opposed to the proactive approach, the <em>passive </em>approach is a lazier strategy whereby the new item is first inserted and then we check the parent for violation, potentially having to perform splitting all the way up to the root node.

<strong>Problem 1.f.          </strong>Prove that:

<ul>

 <li>If node <em>w </em>is split, then it satisfies the preconditions of the split routine</li>

 <li>Before <em>x </em>is inserted into <em>w </em>(at the last step), there are <em>&lt; b </em>keys in <em>w </em>(so key <em>x </em>can be added)</li>

 <li>All three rules of the (<em>a,b</em>)-tree are satisfied after an insertion</li>

</ul>

<strong>Problem 1.g.             </strong>What is the cost of inserting into an (<em>a,b</em>)-tree with <em>n </em>nodes?

<h2>1.4        merge and share operations</h2>

Deletion in an (<em>a,b</em>)-tree risks having nodes shrink too small. In order to support deletion in an (<em>a,b</em>)-tree without violating rules afterwards, we need to introduce two different operations on nodes, each for handling a separate scenario after removing a key from the tree:

<table width="624">

 <tbody>

  <tr>

   <td width="262"><strong>Scenario</strong></td>

   <td colspan="2" width="100"><strong>Operation</strong></td>

   <td width="262"><strong>Algorithm</strong></td>

  </tr>

  <tr>

   <td width="262">•    <em>y </em>and <em>z </em>are siblings•    Have <em>&lt; b </em>− 1 keys together</td>

   <td width="77">merge(<em>y</em>,</td>

   <td width="23"><em>z</em>)</td>

   <td width="262">1.   In parent, delete key <em>v </em>(the key separating the siblings)2.   Add <em>v </em>to the keylist of <em>y</em>3.   In <em>y</em>, merge in <em>z</em>’s keylist and treelist4.   Remove <em>z</em></td>

  </tr>

  <tr>

   <td width="262">•    <em>y </em>and <em>z </em>are siblings•    Have ≥ <em>b </em>− 1 keys together</td>

   <td width="77">share(<em>y</em>,</td>

   <td width="23"><em>z</em>)</td>

   <td width="262">1.   merge(<em>y</em>, <em>z</em>)2.   Split newly combined node using the regular split operation</td>

  </tr>

 </tbody>

</table>

<h2>1.5        Deletion</h2>

Now it is easy to delete an item <em>x </em>from the tree:

<ol>

 <li>Start at node <em>w </em>= <em>root</em></li>

 <li>Repeat until <em>w </em>contains <em>x </em>or <em>w </em>is a leaf:

  <ul>

   <li>If <em>w </em>contains <em>a </em>− 1 keys and <em>z </em>is a sibling of <em>w</em></li>

   <li>If <em>w </em>and <em>z </em>together contain <em>&lt; b </em>− 1 keys,

    <ol>

     <li>merge(<em>w</em>,<em>z</em>)</li>

     <li>Set <em>w </em>to be the newly merged node</li>

    </ol></li>

  </ul></li>

</ol>

<ul>

 <li>Else if <em>w </em>and <em>z </em>together contain ≥ <em>b </em>− 1 keys,</li>

</ul>

<ol>

 <li>share(<em>w</em>,<em>z</em>)</li>

 <li>Set <em>w </em>to be the newly split node whose key range contains <em>x</em></li>

</ol>

<ul>

 <li>Search for <em>x </em>in the keylist of <em>w</em>

  <ul>

   <li>If <em>x </em>is larger than all the keys, set <em>w </em>to be the final child</li>

   <li>Else, look for the first key <em>v<sub>i </sub></em>≥ <em>x </em>and set <em>w </em>to be <em>t<sub>i </sub></em>(subtree with key range ≤ <em>v<sub>i</sub></em>)</li>

  </ul></li>

</ul>

<ol start="3">

 <li>Check for <em>x </em>in <em>w</em>’s keylist,

  <ul>

   <li>If it exists, remove it and return success</li>

   <li>Else, return failure</li>

  </ul></li>

</ol>

Just alike insertion, notice in the deletion procedure outlined above, we also employed a proactive approach to merge/share any nodes we encounter along the way which are at minimum key capacity (i.e. <em>a </em>− 1). With such an approach, we are guaranteed that the parent of the node to undergo merge/share will not be left with too few keys after the removal of the key separating the siblings. On the other hand, the <em>passive </em>approach for deletion first deletes the target key, carry out merge/share on the node (if necessary) and then check the parent for violation, potentially having to perform merge/share all the way up the tree.

<strong>Problem 1.h. </strong>Come up with an example each for merge and share operations, as well as a key deletion. Prove that for a merge or share, the necessary preconditions are satisfied before the operation, and that the three rules of an (<em>a,b</em>)-tree are satisfied afterwards. Thereafter, prove that after a delete operation, the three rules of an (<em>a,b</em>)-tree are satisfied. What is the cost of deleting from an (<em>a,b</em>)-tree?

<h3><strong>Problem 2.              External Memory (or why we care about B-Trees)</strong></h3>

In general, large amounts of data are stored on disk. And searching big data is where efficiency is most important and (<em>a,b</em>)-trees shine. In general, data is stored on disk in blocks, e.g., <em>B</em><sub>1</sub><em>,B</em><sub>2</sub><em>,…,B<sub>m</sub></em>. Each block stores a chunk of memory of size <em>B</em>. You can think of each block as an array of size <em>B</em>.

Note that when you want to access a memory location in some block <em>B<sub>j</sub></em>, the entire block is read into memory. You can assume that your memory can hold <em>M </em>blocks at a time. Transfering a block from disk to memory is expensive: it requires spinning up the harddrive platter, moving the arm to the right track (using, e.g., the elevator algorithm!), and reading the disk while the platter spins. By contrast, reading the block once it is stored in memory is almost instantaneous (by comparison), e.g., many, many orders of magnitude faster. (See Table 2)

To estimate the cost of searching data on disk, let us only count the number of blocks we have to move from disk to memory. (We can ignore the cost of accessing a block once it is in memory. If we run out of space in memory (e.g., we need more than <em>M </em>blocks), then one of the blocks we do not need should be kicked out. For today, let’s assume that never happens.

CPU caches are housed within the CPU which means they are blazing fast memory units. This is why they sit at the top of the memory hierarchy. CPU caches are organized in levels, with L1 being the fastest and L3 being the slowest. To keep our discussions simple, we will be ignoring them today.

Notice that accessing the disk, especially if it is a magnetic disk (as is typical for 100TB disks), is 100,000 times slower than accessing memory. Just as a numerical example, if 90% of your data is

<table width="561">

 <tbody>

  <tr>

   <td width="120"><strong>Memory unit</strong></td>

   <td width="110"><strong>Size</strong></td>

   <td width="90"><strong>Block size</strong></td>

   <td width="242"><strong>Access time (clock cycles)</strong></td>

  </tr>

  <tr>

   <td width="120">L1 cache</td>

   <td width="110">64KB</td>

   <td width="90">64B</td>

   <td width="242">4</td>

  </tr>

  <tr>

   <td width="120">L2 cache</td>

   <td width="110">256KB</td>

   <td width="90">64B</td>

   <td width="242">10</td>

  </tr>

  <tr>

   <td width="120">L3 cache</td>

   <td width="110">up to 40MB</td>

   <td width="90">64B</td>

   <td width="242">40–74</td>

  </tr>

  <tr>

   <td width="120">Main memory</td>

   <td width="110">128GB</td>

   <td width="90">16KB</td>

   <td width="242">200-350</td>

  </tr>

  <tr>

   <td width="120">(Magnetic) Disk</td>

   <td width="110">Arbitrarily big</td>

   <td width="90">16KB</td>

   <td width="242">20,000,000 (An SSD is only 20,000)</td>

  </tr>

 </tbody>

</table>

<strong>Table 2: </strong>Some typical numbers (from the Haswell architecture data sheet)

found in the L1 cache, 8% of your data is found in the L2 cache, and 2% of your data is in main memory, that means that you will be spending 57% of your time waiting on main memory. If you have to get any data from disk, you will be spending 99% of your time waiting for the disk!

We will be focusing on data stored on disk, and counting the number of block transfers between disk and main memory.

<strong>Problem 2.a. </strong>Assume your data is stored on disk. Your data is a sorted array of size <em>n</em>, and spans many blocks. What is the number of block transfers needed (i.e. cost) of doing a <em>linear search </em>for an item? Leave your answer in terms of <em>n </em>and <em>B</em>.

<strong>Problem 2.b. </strong>Assume your data is stored on disk. Your data is a sorted array of size <em>n</em>, and spans many blocks. What is the number of block transfers needed (i.e. cost) of doing a <em>binary search </em>for an item? Leave your answer in terms of <em>n </em>and <em>B</em>.

Now, imagine you are storing your data in a B-tree. (Notice that you might choose <em>a </em>= <em>B/</em>2 and <em>b </em>= <em>B</em>, or <em>a </em>= <em>B/</em>4 and <em>b </em>= <em>B/</em>2, etc., depending on how you want to optimize.)

Notice that each node in your B-tree can now be stored in <em>O</em>(1) blocks, for example, one block stores the key list, one block stores the subtree links, and one block stores the other information (e.g., the parent pointer, pointers to the two other blocks, and any other auxiliary information).

<strong>Problem 2.c. </strong>What is the cost of searching a keylist in a B-tree node? What is the cost of splitting a B-tree node? What is the cost of merging or sharing B-tree nodes?

<strong>Problem 2.d. </strong>What is the cost of searching a B-tree? What is the cost of inserting or deleting in a B-tree?

<h3><strong>Problem 3.          (Extra challenge problems)</strong></h3>

<strong>Problem 3.a. </strong>Imagine you have two <em>B</em>-tree <em>T</em><sub>1 </sub>and <em>T</em><sub>2</sub>, where every element in <em>T</em><sub>1 </sub>is less than every element in <em>T</em><sub>2</sub>. How could you merge these into a single (<em>a,b</em>)-tree. (Remember, the resulting tree has to satisfy the three rules of an <em>B</em>-tree.) What is the cost of this operation?

<strong>Problem 3.b. </strong>Conversely, what if you have an (<em>a,b</em>)-tree <em>T </em>and a key <em>x </em>and want to divide it into two trees <em>T</em><sub>1 </sub>and <em>T</em><sub>2 </sub>where every element ≤ <em>x </em>is in tree <em>T</em><sub>1 </sub>and every element <em>&gt; x </em>is in tree <em>T</em><sub>2</sub>? How would you do this efficiently? What is the cost of this operation?