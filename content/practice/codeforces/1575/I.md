--- 
tựa đề: "CF 1575I - Ảo tưởng về sa mạc" 
Mô tả: "Mê cung là một cái cây. Mỗi phòng đều có một giá trị$ai$, được gọi là tỷ lệ ảo ảnh của nó. Di chuyển qua một cạnh$(x,y)$chi phí$$max( Queries either change the value of one room or ask for the minimum energy needed to travel between two rooms." date: "2026-06-10T10:56:33+07:00" tags: ["codeforces", "competitive-programming", "data-structures", "trees"] categories: ["algorithms"] codeforces_contest: 1575 codeforces_index: "I" codeforces_contest_name: "COMPFEST 13 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred)" rating: 2300 weight: 1575 solve_time_s: 43 verified: false draft: false --- [CF 1575I - Illusions of the Desert](https://codeforces.com/problemset/problem/1575/I) **Rating:** 2300   **Tags:** data structures, trees   **Solve time:** 43s   **Verified:** no   ## Solution ## Problem Understanding The labyrinth is a tree. Every room has a value $a_i$, called its illusion rate. Moving through an edge $(x,y)$ costs$$\max(|a_x+a_y|,\ |a_x-a_y|).$$
Queries either change the value of one room or ask for the minimum energy needed to travel between two rooms.
Because the graph is a tree, there is exactly one path between any two rooms. A type 2 query is simply asking for the total edge cost along that unique path.
The first challenge is understanding the edge cost formula. For any two numbers $p$ and $q$,
$$\max(|p+q|,\ |p-q|)=|p|+|q|.$$
One way to see this is to assume $|p|\ge |q|$. Then $p+q$ and $p-q$ have absolute values whose maximum is exactly $|p|+|q|$.
After this simplification, every edge $(x,y)$ has weight
$$|a_x|+|a_y|.$$ 
Các giá trị thay đổi trong quá trình cập nhật, do đó trọng số của cạnh là động. 
Các ràng buộc rất lớn: cả hai$n$Và$q$có thể đạt được$10^5$. Một truy vấn có thể liên quan đến một đường dẫn chứa$O(n)$đỉnh. Việc tính toán lại toàn bộ đường dẫn cho mỗi truy vấn sẽ dẫn đến kết quả gần đúng$10^{10}$trong trường hợp xấu nhất, vượt xa giới hạn 3 giây. Chúng tôi cần một cái gì đó gần với thời gian logarit cho mỗi truy vấn. 
Một số trường hợp dễ bỏ sót đáng được quan tâm. 
Xem xét một truy vấn từ một nút đến chính nó:```
1
|
(only one node)
2 1 1
```Không có cạnh nào được đi qua, vì vậy câu trả lời là 0. Bất kỳ công thức nào tính tổng các trọng số của đường đi một cách mù quáng đều phải xử lý điều này một cách chính xác.
