1. Tokenization aur Inverted Index (Deep Dive)
Tokenization:
•	Tokenization ka process ElasticSearch ke indexing system mein raw data ko useful terms mein todne ka kaam karta hai. Yeh text pre-processing ka first step hota hai.
•	Example:
o	Agar document ka content hai: "ElasticSearch is fast"
o	Tokenization ke baad yeh terms banenge: ["ElasticSearch", "is", "fast"]
o	Tokenization se stop words (like "is") ko remove karna optional ho sakta hai, depending on analyzer settings.
Inverted Index:
•	Tokenization ke baad, har ek token ko inverted index mein map kiya jata hai. Inverted index ek reverse mapping hota hai, jo terms ko un documents ke IDs ke saath link karta hai jahan woh term appear hota hai.
•	Inverted Index Example:
o	Documents:
1.	"ElasticSearch is fast"
2.	"Elasticsearch is scalable"
3.	"ElasticSearch for data search"
o	Inverted Index would look like:
Term	Document IDs
ElasticSearch	[1, 2, 3]
is	[1, 2]
fast	[1]
scalable	[2]
for	[3]
data	[3]
search	[3]
o	Inverted Index ka kaam: Jab aap query karte hain, ElasticSearch ko directly pata hota hai ki kis term ke saath kis document ka match hai, aur woh search operations ko efficient banaata hai.
________________________________________
2. Distributed Search Engine: Why Distributed?
ElasticSearch ek distributed system hai, jisme multiple nodes data ko store karte hain. Distributed system ka concept ElasticSearch ko horizontally scale karne mein madad karta hai.
Distributed System ka Faida:
•	Scaling: ElasticSearch ko horizontally scale kiya ja sakta hai, jisme aap multiple machines ya nodes add karke data processing aur storage ko efficiently handle kar sakte hain.
•	Fault Tolerance: Agar kisi ek node ka failure ho jata hai, toh data replication aur sharding ki wajah se ElasticSearch ko koi major loss nahi hota, aur search continue rehti hai.
Node and Cluster:
•	Node ek ElasticSearch ka instance hota hai. Har node cluster ka part hota hai aur data ko store karta hai.
•	Cluster ek logical grouping hoti hai nodes ki. Ek ElasticSearch cluster mein multiple nodes ho sakte hain jo apne-apne shards ko manage karte hain.
How Does It Work:
•	Jab aap ElasticSearch me data index karte hain, toh woh data cluster ke andar primary shards mein distribute hota hai, aur replica shards ki copies bhi banti hain taaki redundancy ho sake.
________________________________________
3. Shards: Primary and Replica
Primary Shards:
•	ElasticSearch ke index ko primary shards mein divide kiya jata hai.
•	Har primary shard ek independent unit hota hai jo apne document ko store karta hai aur queries ko process karta hai.
Replica Shards:
•	Replica shards ek backup hote hain primary shards ke. Agar primary shard fail hota hai, toh replica shard automatically active ho jata hai.
•	Yeh system fault tolerance aur high availability ko ensure karta hai.
Shard Allocation Example:
•	Agar aap ek index create karte hain with 3 primary shards aur 2 replicas, toh ElasticSearch data ko 3 primary shards mein distribute karega aur 6 total shards (3 primary + 3 replica) create karega.
________________________________________
4. Relevance Scoring (TF-IDF, BM25)
TF-IDF (Term Frequency-Inverse Document Frequency):
•	TF-IDF ek scoring algorithm hai jo text search mein use hota hai. Yeh term ke importance ko measure karta hai ek document ke andar aur across the entire corpus.
TF (Term Frequency):
•	Ek document mein koi term kitni baar appear hota hai. Jaise agar ek document mein "elastic" term 5 baar appear hota hai aur document mein total 100 terms hain, toh TF = 5/100 = 0.05.
IDF (Inverse Document Frequency):
•	Yeh measure karta hai ki term kitni documents mein appear hota hai. Agar term zyada documents mein appear ho raha hai, toh uska IDF score kam hoga. Agar term kam documents mein appear ho raha hai, toh IDF score zyada hoga.
Formula:
IDF(t)=log⁡(Ndf(t))\text{IDF}(t) = \log\left(\frac{N}{df(t)}\right)IDF(t)=log(df(t)N)
Where:
•	NNN = total number of documents
•	df(t)df(t)df(t) = number of documents containing term ttt
TF-IDF Final Score:
TF-IDF=TF(t)×IDF(t)\text{TF-IDF} = \text{TF}(t) \times \text{IDF}(t)TF-IDF=TF(t)×IDF(t)
Yeh formula determine karta hai ki koi document kitna relevant hai kisi given term ke liye.
BM25 (Best Matching 25):
•	BM25 ek enhancement hai TF-IDF ka, jo better relevance score provide karta hai.
•	Yeh term frequency saturation ko consider karta hai. Matlab, jab kisi term ka frequency bohot high ho jata hai, toh uski additional frequency ka weight zyada nahi hota.
BM25 Example:
BM25=∑i=1nIDF(qi)×f(qi,D)×(k1+1)f(qi,D)+k1×(1−b+b×∣D∣avg_doc_length)\text{BM25} = \sum_{i=1}^{n} \text{IDF}(q_i) \times \frac{f(q_i, D) \times (k_1 + 1)}{f(q_i, D) + k_1 \times (1 - b + b \times \frac{|D|}{\text{avg\_doc\_length}})}BM25=i=1∑nIDF(qi)×f(qi,D)+k1×(1−b+b×avg_doc_length∣D∣)f(qi,D)×(k1+1)
Where:
•	k1k_1k1, bbb = parameters that control term frequency and document length normalization.
BM25 ko use karte waqt, ElasticSearch relevancy scoring ko better tune karta hai for modern search use cases.
________________________________________
5. Zen Discovery: Automatic Node Discovery and Cluster Formation
Zen Discovery:
•	Zen Discovery ElasticSearch ka internal mechanism hai jo cluster ke nodes ko discover karta hai aur unke beech communication establish karta hai.
•	Agar kisi node ko fail ho jata hai ya naya node cluster me join hota hai, toh Zen Discovery usko automatically handle karta hai.
How It Works:
•	Jab ElasticSearch start hota hai, toh har node apni Zen Discovery process ko initiate karta hai. Is process ke through, nodes apne aap ko cluster ke baaki nodes ke saath connect karte hain.
•	Yeh cluster formation aur failover handling ko efficiently manage karta hai.
________________________________________
6. Real-Time Search
ElasticSearch real-time search provide karta hai, jiska matlab hai ki jab aap data index karte hain, woh data immediately searchable ho jata hai.
How It Works:
•	Lucene ke underlying architecture ka use karke, ElasticSearch indexing ko optimize karta hai. Segments mechanism ke through, newly indexed data ko quickly searchable banaaya jata hai.
•	Yeh near real-time search hai, jisme data typically within 1 second searchable ho jata hai.
Example:
•	Agar aap "ElasticSearch" term ko index karte hain, toh uska search result turant available hota hai. Yeh search delay ko avoid karta hai aur aapko fast search response milta hai.
________________________________________
7. Caching Mechanism
ElasticSearch mein query caching ka mechanism hota hai jo repeated queries ko speed up karta hai.
Types of Caching:
•	Query Cache: Agar kisi query ka result baar-baar poocha jata hai, toh woh result memory mein cache ho jata hai. Next time jab same query execute hoti hai, toh cached result immediately return hota hai.
•	Filter Cache: Filters ke liye bhi ElasticSearch cache maintain karta hai taaki repeated filters ko optimize kiya ja sake.
Cache Mechanism Example:
•	Agar aap search term "ElasticSearch" ke liye query run karte hain aur wo repeated query aap baar-baar run karte hain, toh woh query cache mein store ho jata hai.
Cache ka Benefit:
•	Caching query performance ko improve karta hai aur response time ko significantly reduce karne mein madad karta hai.

