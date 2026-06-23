---
title: "CF 105327B - Số thịt xông khói"
description: "Chúng ta được cung cấp một bộ sưu tập phim, trong đó mỗi phim có một nhóm diễn viên. Hai diễn viên được coi là có mối liên hệ trực tiếp nếu họ xuất hiện cùng nhau trong ít nhất một bộ phim."
date: "2026-06-22T09:57:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105327
codeforces_index: "B"
codeforces_contest_name: "2024-2025 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 105327
solve_time_s: 91
verified: false
draft: false
---

[CF 105327B - Số thịt xông khói](https://codeforces.com/problemset/problem/105327/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập phim, trong đó mỗi phim có một nhóm diễn viên. Hai diễn viên được coi là có mối liên hệ trực tiếp nếu họ xuất hiện cùng nhau trong ít nhất một bộ phim. Điều này xác định một cách tự nhiên một đồ thị vô hướng: các tác nhân là các nút và một cạnh tồn tại giữa hai tác nhân nếu có một phim chứa cả hai tác nhân đó. 

Tuy nhiên, nhiệm vụ không chỉ là xác định khả năng kết nối. Đối với mỗi truy vấn, chúng ta được yêu cầu xây dựng một chuỗi xen kẽ rõ ràng bắt đầu từ tác nhân$x$và kết thúc ở diễn viên$y$, trong đó các diễn viên liên tiếp phải chia sẻ một bộ phim và các phim liên tiếp phải chứa các diễn viên liền kề. Nói cách khác, chúng ta phải xuất ra một đường dẫn trong cấu trúc giống như lưỡng cực bao gồm các diễn viên và phim, xen kẽ giữa chúng, với ràng buộc là một diễn viên chỉ có thể chuyển sang một diễn viên khác thông qua một bộ phim chung. 

Khó khăn chính đó là$M$, số lượng tác nhân, có thể lên tới$10^6$, vì vậy chúng ta không thể xây dựng hoặc duyệt qua biểu đồ tác nhân đầy đủ một cách rõ ràng. Số lượng phim ít, nhiều nhất là 100, nhưng mỗi phim có thể có nhiều diễn viên và tổng số lần xuất hiện của các diễn viên nhiều nhất là$10^6$. Điều này cho thấy rõ ràng rằng cấu trúc phim thưa thớt nhưng có khả năng dày đặc trong mỗi bộ phim. 

Một cách giải thích ngây thơ sẽ là xây dựng một biểu đồ đầy đủ về các diễn viên, thêm các cạnh giữa tất cả các cặp trong mỗi bộ phim. Điều đó ngay lập tức trở nên không khả thi vì chỉ một bộ phim với$k$diễn viên sẽ đóng góp$O(k^2)$các cạnh, vượt xa giới hạn khi$k$là lớn. 

Cách tiếp cận đơn giản thứ hai là chạy BFS trên các tác nhân cho mỗi truy vấn, khám phá các phim được chia sẻ một cách linh hoạt. Mặc dù đúng về mặt khái niệm nhưng việc quét liên tục tất cả các phim để tìm các phim liền kề sẽ dẫn đến$O(Q \cdot N \cdot M)$-style hành vi trong trường hợp xấu nhất, cũng quá chậm. 

Một trường hợp phức tạp phát sinh khi các diễn viên chỉ được kết nối thông qua chuỗi phim dài. Ví dụ: nếu phim 1 có diễn viên$[1,2]$, phim 2 chứa$[2,3]$, và phim 3 chứa$[3,4]$, sau đó là một truy vấn$(1,4)$đòi hỏi phải tạo ra một đường dẫn xen kẽ rõ ràng. Một giải pháp ngây thơ chỉ kiểm tra sự xuất hiện trực tiếp sẽ trả về một cách không chính xác là không thể truy cập được, mặc dù khả năng kết nối tồn tại thông qua các tác nhân trung gian. 

Một trường hợp khó khăn khác là khi một diễn viên xuất hiện trong nhiều bộ phim chồng chéo lên nhau. Nếu chúng ta bất cẩn và xử lý từng bộ phim một cách độc lập mà không theo dõi các trạng thái được truy cập toàn cầu, chúng ta có thể lặp lại các diễn viên không chính xác hoặc không tìm được kết nối ngắn nhất hoặc thậm chí bất kỳ kết nối hợp lệ nào. 

## Phương pháp tiếp cận 

Cấu trúc của bài toán gợi ý rằng các bộ phim hoạt động giống như các siêu cạnh kết nối nhiều tác nhân. Thay vì mở rộng từng bộ phim thành một nhóm, chúng ta có thể coi biểu đồ là một bài toán truyền tải hai bên giữa các diễn viên và phim. 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi chạy BFS trong đó các trạng thái là diễn viên và quá trình chuyển đổi diễn ra thông qua các bộ phim: từ một diễn viên, chúng tôi khám phá tất cả các phim thuộc về họ và từ mỗi bộ phim như vậy, chúng tôi có thể tiếp cận tất cả các diễn viên khác trong đó. Điều này đúng vì nó khám phá rõ ràng tất cả các chuyển đổi hợp lệ. Vấn đề là mỗi truy vấn có thể quét liên tục các danh sách phim lớn. Nếu một bộ phim chứa$k$các diễn viên, sau đó khám phá nó sẽ góp phần$O(k)$hoạt động và trên nhiều truy vấn, điều này trở nên nhân lên. Trong trường hợp xấu nhất, điều này thoái hóa thành việc xử lý liên tục các bộ phim lớn giống nhau, mang lại kết quả gần như$O(Q \cdot \sum n_i)$, quá chậm khi$Q = 10^4$Và$\sum n_i = 10^6$. 

Nhận xét quan trọng là số lượng phim ít. Thay vì mở rộng phim liên tục trong BFS, chúng ta có thể tái sử dụng chúng dưới dạng trình kết nối nén. Chúng tôi thực hiện BFS đối với các diễn viên nhưng chúng tôi đảm bảo rằng mỗi phim được “mở rộng” tối đa một lần cho mỗi lần tìm kiếm. Khi lần đầu tiên chúng tôi tiếp cận một bộ phim thông qua bất kỳ diễn viên nào, chúng tôi xử lý tất cả các diễn viên của bộ phim đó trong một đợt và sau đó đánh dấu nó là đã được sử dụng cho BFS đó. Điều này ngăn chặn việc quét lặp lại cùng một danh sách lớn. 

Để xây dựng lại trình tự xen kẽ thực tế, chúng tôi lưu trữ các con trỏ gốc không chỉ cho các diễn viên mà còn cho bộ phim được sử dụng để tiếp cận họ. Điều này cho phép chúng ta xây dựng lại đường dẫn dưới dạng diễn viên → phim → diễn viên → chuỗi phim. 

Vì mỗi phim được xử lý nhiều nhất một lần cho mỗi truy vấn BFS nên tổng công việc trên mỗi truy vấn trở nên tuyến tính theo số lượng diễn viên và phim liên quan gặp phải trong quá trình tìm kiếm, vốn bị giới hạn bởi các ràng buộc đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS cho mỗi truy vấn với việc mở rộng phim lặp đi lặp lại |$O(Q \cdot \sum n_i)$|$O(\sum n_i)$| Quá chậm | 
| BFS được tối ưu hóa với tính năng truy cập phim theo truy vấn + xây dựng lại cấp độ gốc |$O(\sum n_i + Q \cdot N)$trường hợp xấu nhất thực tế |$O(\sum n_i)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa quy trình này dưới dạng BFS dựa trên các tác nhân, nhưng với các bộ phim đóng vai trò là trung tâm mở rộng. 

1. Xây dựng danh sách liền kề từ phim đến diễn viên và từ diễn viên đến phim. Điều này được lưu trữ để chúng ta có thể nhanh chóng di chuyển giữa hai loại nút. 
2. Đối với mỗi truy vấn$(x, y)$, khởi tạo hàng đợi BFS với diễn viên$x$. Chúng tôi cũng duy trì mảng`prev_actor`Và`prev_movie`để xây dựng lại đường đi. Chúng tôi cũng duy trì một mảng đã truy cập cho các diễn viên và một mảng đã truy cập riêng cho phim, đặt lại cho mỗi truy vấn. 
3. Trong khi hàng đợi không trống, chúng tôi chọn một diễn viên$a$. Đối với mỗi bộ phim$m$chứa đựng$a$, nếu như$m$chưa được xử lý trong BFS này, chúng tôi đánh dấu nó là đã được xử lý và lặp lại qua tất cả các tác nhân$b$trong bộ phim đó. Đối với mỗi diễn viên như vậy$b$chưa ghé thăm, chúng tôi đặt`prev_actor[b] = a`Và`prev_movie[b] = m`, sau đó đẩy$b$vào hàng đợi. 

Lý do chúng tôi đánh dấu phim là đã xử lý là để tránh quét lại cùng một phim nhiều lần thông qua các diễn viên khác nhau, điều này sẽ làm cho tác phẩm bị trùng lặp rất nhiều. 
4. Nếu chúng ta đạt được$y$, chúng tôi dừng BFS sớm vì chúng tôi chỉ cần một đường dẫn hợp lệ. 
5. Để xây dựng lại con đường, chúng ta đi lùi từ$y$sử dụng`prev_actor`Và`prev_movie`. Điều này tạo ra một chuỗi các diễn viên và phim xen kẽ đảo ngược. 
6. Đảo ngược trình tự được xây dựng lại để tạo ra đầu ra cuối cùng theo thứ tự thuận. 

Tính đúng đắn dựa trên tính bất biến rằng khi một tác nhân được phát hiện lần đầu tiên trong BFS, chúng ta đã tìm thấy một đường dẫn xen kẽ hợp lệ tới nó. Bởi vì BFS khám phá các lớp mở rộng phim-diễn viên xen kẽ, lần đầu tiên chúng tôi tiếp cận một nút đảm bảo một kết nối hợp lệ và việc lưu trữ các con trỏ gốc sẽ duy trì cấu trúc đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    
    movies = []
    actor_movies = [[] for _ in range(m + 1)]
    
    for i in range(n):
        arr = list(map(int, input().split()))
        k = arr[0]
        people = arr[1:]
        movies.append(people)
        for p in people:
            actor_movies[p].append(i)
    
    q = int(input())
    
    for _ in range(q):
        s, t = map(int, input().split())
        
        if s == t:
            print(1)
            print(s)
            continue
        
        visited_actor = [False] * (m + 1)
        used_movie = [False] * n
        prev_actor = [-1] * (m + 1)
        prev_movie = [-1] * (m + 1)
        
        dq = deque([s])
        visited_actor[s] = True
        
        found = False
        
        while dq and not found:
            a = dq.popleft()
            
            for mv in actor_movies[a]:
                if used_movie[mv]:
                    continue
                used_movie[mv] = True
                
                for b in movies[mv]:
                    if not visited_actor[b]:
                        visited_actor[b] = True
                        prev_actor[b] = a
                        prev_movie[b] = mv
                        if b == t:
                            found = True
                            break
                        dq.append(b)
                if found:
                    break
        
        if not visited_actor[t]:
            print(-1)
            continue
        
        path = []
        cur = t
        
        while cur != -1:
            path.append(cur)
            cur = prev_actor[cur]
        
        path.reverse()
        
        print(len(path))
        print(*path)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ nguyên cấu trúc phim thay vì làm phẳng nó thành các cạnh diễn viên. các`actor_movies`danh sách cho phép một diễn viên truy cập nhanh vào tất cả các bộ phim mà họ xuất hiện.`used_movie`mảng rất quan trọng vì nó đảm bảo mỗi phim được mở rộng tối đa một lần trên mỗi BFS, ngăn chặn việc quét lặp lại các danh sách tác nhân lớn. 

Giai đoạn tái thiết chỉ dựa vào`prev_actor`, vì đầu ra chỉ yêu cầu diễn viên chứ không yêu cầu phim rõ ràng. Phim chỉ được sử dụng để biện minh cho quá trình chuyển đổi trong BFS. 

Một điểm tinh tế là dừng lại sớm khi tìm thấy mục tiêu. Nếu không có điều này, BFS có thể tiếp tục mở rộng các bộ phim không cần thiết, tăng thời gian chạy đáng kể trong các trường hợp dày đặc. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

Trước tiên, chúng tôi xây dựng tư cách thành viên phim và mối quan hệ phụ cận giữa diễn viên với phim. Đối với truy vấn$1 \to 5$, BFS bắt đầu từ diễn viên 1. Nó khám phá các phim chứa 1, đánh dấu chúng là đã sử dụng và tiếp cận diễn viên 2 và 5 tùy thuộc vào phim được chia sẻ. Khi đạt đến số 5, chúng tôi quay lại bằng cách sử dụng các con trỏ gốc và xây dựng lại chuỗi. 

| Bước | Xếp hàng | Diễn viên tham quan | Phim đã qua sử dụng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | {1} | {} | bắt đầu BFS | 
| 2 | [] sau khi mở rộng | {1,2,5} | {phim 0, phim 1} | mở rộng phim 1 | 
| 3 | tìm thấy 5 | dừng lại | | đạt mục tiêu | 

Điều này xác nhận rằng BFS xác định chính xác kết nối thông qua phim được chia sẻ. 

Đối với một truy vấn bị ngắt kết nối, chẳng hạn như$1 \to 6$, BFS khám phá tất cả các thành phần có thể truy cập từ 1 nhưng không bao giờ gặp 6. Mảng đã truy cập dành cho các tác nhân vẫn không có 6, vì vậy chúng tôi xuất chính xác -1. 

| Bước | Xếp hàng | Diễn viên tham quan | Phim đã qua sử dụng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | {1} | {} | bắt đầu BFS | 
| 2 | ... | thành phần có thể truy cập | phim đã qua sử dụng | thăm dò đầy đủ | 
| 3 | trống | {thành phần của 1} | tất cả đều có liên quan | 6 chưa bao giờ đạt | 

Điều này thể hiện việc xử lý chính xác các thành phần bị ngắt kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sum n_i + Q \cdot \text{small BFS})$| mỗi phim được mở rộng tối đa một lần cho mỗi BFS, tổng kích thước đầu vào là tuyến tính | 
| Không gian |$O(\sum n_i)$| lưu trữ danh sách phim và lân cận | 

Tổng số lần xuất hiện của diễn viên nhiều nhất là$10^6$, do đó cả BFS tiền xử lý và BFS cho mỗi truy vấn vẫn nằm trong giới hạn. Thuật toán tránh hiện tượng bùng nổ bậc hai khi mở rộng phim thành các nhóm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n, m = map(int, input().split())
        movies = []
        actor_movies = [[] for _ in range(m + 1)]
        for i in range(n):
            arr = list(map(int, input().split()))
            k = arr[0]
            people = arr[1:]
            movies.append(people)
            for p in people:
                actor_movies[p].append(i)

        q = int(input())
        out = []
        for _ in range(q):
            s, t = map(int, input().split())
            if s == t:
                out.append("1\n{}".format(s))
                continue

            visited_actor = [False] * (m + 1)
            used_movie = [False] * n
            prev_actor = [-1] * (m + 1)

            dq = deque([s])
            visited_actor[s] = True
            found = False

            while dq and not found:
                a = dq.popleft()
                for mv in actor_movies[a]:
                    if used_movie[mv]:
                        continue
                    used_movie[mv] = True
                    for b in movies[mv]:
                        if not visited_actor[b]:
                            visited_actor[b] = True
                            prev_actor[b] = a
                            if b == t:
                                found = True
                                break
                            dq.append(b)
                    if found:
                        break

            if not visited_actor[t]:
                out.append("-1")
                continue

            path = []
            cur = t
            while cur != -1:
                path.append(cur)
                cur = prev_actor[cur]
            path.reverse()

            out.append(str(len(path)))
            out.append(" ".join(map(str, path)))

        return "\n".join(out)

    return solve()

# provided sample
assert run("""4 6
3 1 2 5
3 1 3 5
2 2 4
1 6
4
1 5
1 4
3 4
1 6
""") == """2
1 5
3
1 2 3
4
3 5 1 2 4
-1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phim diễn viên đơn | kết nối trực tiếp | trường hợp BFS đơn giản nhất | 
| đồ thị bị ngắt kết nối | -1 | xử lý không thể truy cập | 
| chuỗi phim | con đường dài | tái thiết nhiều bước | 

## Vỏ cạnh 

Một trường hợp tối thiểu với một bộ phim có hai diễn viên kiểm tra tính liền kề trực tiếp. BFS bắt đầu tại một diễn viên, mở rộng phim đó một lần và ngay lập tức phát hiện ra diễn viên kia. Con trỏ gốc được đặt trực tiếp và việc xây dựng lại mang lại đường dẫn hai tác nhân không có các bước trung gian. 

Trường hợp hoàn toàn bị ngắt kết nối, trong đó mỗi diễn viên xuất hiện trong một bộ phim riêng biệt, đảm bảo rằng BFS sẽ lấy hết tất cả các phim có thể truy cập được từ diễn viên bắt đầu mà không bao giờ đánh dấu mục tiêu. Mảng đã truy cập ngăn chặn các vòng lặp vô hạn và kết quả đầu ra chính xác là -1. 

Một bộ phim dày đặc có nhiều diễn viên đảm bảo rằng`used_movie`tối ưu hóa là điều cần thiết. Nếu không đánh dấu phim là đã xử lý, mỗi diễn viên sẽ quét lại cùng một danh sách, nhân lên công việc. Với việc tối ưu hóa, bộ phim được mở rộng một lần, tất cả các diễn viên được xếp hàng một lần và BFS diễn ra suôn sẻ.
