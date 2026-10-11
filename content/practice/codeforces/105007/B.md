---
title: "CF 105007B - Corgi Hike"
description: "Chúng ta có địa hình một chiều được mô tả bằng một loạt các độ cao. Mỗi chỉ mục đại diện cho một vị trí dọc theo một đường dẫn và mỗi vị trí có một giá trị độ cao."
date: "2026-06-28T03:10:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105007
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 03-01-24 Div. 2 (Beginner)"
rating: 0
weight: 105007
solve_time_s: 175
verified: false
draft: false
---

[CF 105007B - Corgi Hike](https://codeforces.com/problemset/problem/105007/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có địa hình một chiều được mô tả bằng một loạt các độ cao. Mỗi chỉ mục đại diện cho một vị trí dọc theo một đường dẫn và mỗi vị trí có một giá trị độ cao. Đối với bất kỳ hai vị trí được truy vấn$i$Và$j$, chúng ta phải quyết định xem liệu có tồn tại đường thẳng nhìn giữa chúng hay không theo quy tắc cản trở cụ thể. 

Quy tắc nói rằng đoạn giữa$i$Và$j$chặn tầm nhìn nếu tồn tại bất kỳ vị trí trung gian nào có độ cao ít nhất bằng điểm cao hơn của hai điểm cuối. Nói cách khác, nếu chúng ta định nghĩa$H = \max(e_i, e_j)$, thì mọi chỉ số đều nằm giữa$i$Và$j$phải có độ cao nhỏ hơn$H$để hai điểm cuối có thể nhìn thấy được. 

Mỗi truy vấn là độc lập. Chúng tôi không cập nhật địa hình mà chỉ kiểm tra điều kiện tầm nhìn nhiều lần. 

Các ràng buộc cho phép lên đến$N = 100{,}000$vị trí nhưng chỉ lên đến$q = 100$truy vấn. Sự mất cân bằng này là gợi ý cơ cấu quan trọng. Bất kỳ giải pháp nào xử lý trước một cách mạnh mẽ hoặc sử dụng công việc logarit cho mỗi truy vấn đều có thể chấp nhận được, trong khi mọi giải pháp tuyến tính cho mỗi truy vấn vẫn trôi qua một cách thoải mái, vì$100 \times 100{,}000 = 10^7$hoạt động an toàn trong Python. 

Việc quét bậc hai trên tất cả các cặp vị trí được loại trừ để xử lý trước, nhưng việc quét dọc theo một phân đoạn cho mỗi truy vấn vẫn khả thi. 

Một trường hợp tế nhị phát sinh khi$i = j$. Trong tình huống đó, không có điểm trung gian nên khả năng hiển thị phải luôn đúng. 

Một trường hợp góc khác là khi độ cao tối đa giữa các điểm cuối không phải là duy nhất. Ví dụ: nếu cả hai điểm cuối đều bằng nhau và một số điểm bên trong khớp với cùng độ cao đó thì khả năng hiển thị phải không thành công vì điều kiện cho phép “lớn hơn hoặc bằng” chứ không phải lớn hơn hoàn toàn. Một giải pháp ngây thơ chỉ kiểm tra các giá trị bên trong lớn hơn một cách nghiêm ngặt sẽ chấp nhận sai các trường hợp như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force đánh giá từng truy vấn một cách độc lập bằng cách quét mọi chỉ mục một cách nghiêm ngặt giữa$i$Và$j$. Đối với một truy vấn cố định, chúng tôi tính toán$H = \max(e_i, e_j)$và sau đó kiểm tra xem bất kỳ vị trí trung gian nào có độ cao ít nhất$H$. Nếu có một vị trí như vậy, câu trả lời là “không”, nếu không thì “có”. 

Cách tiếp cận này đúng vì nó trực tiếp thực hiện định nghĩa về khả năng hiển thị. Vấn đề là chi phí. Trong trường hợp xấu nhất, một truy vấn sẽ kéo dài gần như toàn bộ mảng, do đó mỗi truy vấn sẽ tốn$O(N)$. Với tối đa 100 truy vấn, tổng chi phí sẽ trở thành$O(Nq) = 10^7$, là đường biên nhưng vẫn được chấp nhận trong Python đối với các vòng lặp chặt chẽ, nhưng vẫn để lại rất ít lợi nhuận cho chi phí chung. 

Chúng ta có thể tránh chi phí không cần thiết bằng cách nhận ra rằng cấu trúc của vấn đề không yêu cầu tiền xử lý hoặc cấu trúc dữ liệu nâng cao. Mảng là tĩnh và các truy vấn độc lập, do đó, việc quét tuyến tính đơn giản nhất cho mỗi truy vấn đã là các ràng buộc nhất định tối ưu. 

Không cần cây phân đoạn hoặc cấu trúc RMQ vì có rất ít truy vấn. Tối ưu hóa chính mang tính khái niệm: tránh sử dụng kỹ thuật quá mức và trực tiếp thực hiện điều kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quét Brute Force cho mỗi truy vấn |$O(Nq)$|$O(1)$| Đã chấp nhận | 
| Cây phân đoạn / RMQ |$O((N+q)\log N)$|$O(N)$| Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Đối với mỗi truy vấn$(i, j)$, chuẩn hóa khoảng thời gian để chúng ta luôn lặp từ trái sang phải. Điều này tránh phân nhánh có điều kiện bên trong quá trình quét và đơn giản hóa việc suy luận về các chỉ số trung gian. 
2. Tính toán$H = \max(e_i, e_j)$. Giá trị này thể hiện ngưỡng hiển thị mà bất kỳ điểm chặn nào cũng phải đạt hoặc vượt quá. 
3. Lặp lại tất cả các chỉ số một cách nghiêm ngặt giữa$i$Và$j$. Đối với mỗi vị trí$k$, kiểm tra xem$e_k \ge H$. Nếu điều kiện này được đáp ứng, tầm nhìn sẽ bị chặn và câu trả lời ngay lập tức là “không”. 
4. Nếu quá trình quét hoàn tất mà không tìm thấy bất kỳ vị trí chặn nào thì khoảng thời gian thỏa mãn quy tắc hiển thị, do đó câu trả lời là “có”. 

Điểm quyết định quan trọng là thoát sớm trong quá trình quét. Khi đã tìm thấy chiều cao chặn, việc tiếp tục quét không ảnh hưởng gì đến độ chính xác và chỉ lãng phí thời gian. 

### Tại sao nó hoạt động 

Đối với bất kỳ cặp điểm cuối nào, khả năng hiển thị phụ thuộc hoàn toàn vào việc mức tối đa bên trong có đạt đến ngưỡng hay không$H = \max(e_i, e_j)$. Thuật toán kiểm tra chính xác điều kiện này trên toàn bộ tập hợp các điểm bên trong. Vì mọi vật cản có thể phải xuất hiện trong tập hợp đó và mọi vật cản được phát hiện đều làm mất hiệu lực chính xác khả năng hiển thị, nên quyết định tương đương với định nghĩa chính thức. Không có cấu hình trung gian hoặc thuộc tính chung nào quan trọng ngoài mức kiểm tra tối đa này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    e = list(map(int, input().split()))

    for _ in range(q):
        i, j = map(int, input().split())
        i -= 1
        j -= 1

        if i == j:
            print("yes")
            continue

        if i > j:
            i, j = j, i

        H = e[i] if e[i] > e[j] else e[j]

        ok = True
        for k in range(i + 1, j):
            if e[k] >= H:
                ok = False
                break

        print("yes" if ok else "no")

if __name__ == "__main__":
    solve()
```Giải pháp đọc mảng một lần và trả lời từng truy vấn một cách độc lập. Mỗi truy vấn chuẩn hóa các điểm cuối để hướng quét nhất quán. Ngưỡng$H$được tính toán trực tiếp từ các giá trị điểm cuối và vòng lặp chỉ kiểm tra các chỉ số bên trong. Việc nghỉ sớm là điều cần thiết để tránh việc quét không cần thiết khi đỉnh chặn xuất hiện sớm. 

Phải cẩn thận khi lập chỉ mục vì đầu vào dựa trên 1 nhưng mảng Python dựa trên 0. Trường hợp bình đẳng$i = j$được xử lý rõ ràng để tránh sự mơ hồ của vòng lặp trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
1 3 2 4 1
1 4
2 5
```Đối với truy vấn (1, 4), điểm cuối là 1 và 4, vì vậy$H = 4$. Chúng tôi kiểm tra vị trí 2 và 3. Giá trị của chúng là 3 và 2, cả hai đều dưới 4, do đó khả năng hiển thị được giữ nguyên. 

Đối với truy vấn (2, 5), điểm cuối là 3 và 1, vì vậy$H = 3$. Chúng tôi kiểm tra vị trí 3 và 4, tức là 2 và 4. Vì 4 ít nhất là 3 nên khả năng hiển thị không thành công. 

| Truy vấn | tôi | j | H | Giá trị nội thất | Chặn? | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 4 | 3, 2 | Không | vâng | 
| 2 | 2 | 5 | 3 | 2, 4 | Có | không | 

Điều này xác nhận rằng thuật toán phân biệt chính xác giữa một đỉnh dốc rõ ràng và một đỉnh trung gian chặn tầm nhìn. 

### Ví dụ 2 

đầu vào:```
4 1
5 5 5 5
1 4
```Đây$H = 5$và mọi giá trị bên trong đều là 5, thỏa mãn điều kiện chặn ngay lập tức. Thuật toán tìm chỉ mục bên trong đầu tiên và từ chối truy vấn. 

| k | e[k] | So sánh với H=5 | Trạng thái | 
| --- | --- | --- | --- | 
| 2 | 5 | ≥ | khối | 
| 3 | - | dừng sớm | - | 

Điều này cho thấy rằng sự bình đẳng được coi là sự chặn một cách chính xác, điều này rất cần thiết cho sự đúng đắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Nq)$| Mỗi truy vấn có thể quét toàn bộ phân đoạn của mảng trong trường hợp xấu nhất | 
| Không gian |$O(1)$| Chỉ một số biến cố định được sử dụng ngoài bộ nhớ đầu vào | 

Với$N \le 10^5$Và$q \le 100$, công việc trong trường hợp xấu nhất là xung quanh$10^7$so sánh, nằm trong giới hạn thoải mái đối với Python khi đưa ra các so sánh số nguyên đơn giản và thoát sớm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-style tests
assert run("""5 2
1 3 2 4 1
1 4
2 5
""") == "yes\nno"

# minimum size
assert run("""1 1
10
1 1
""") == "yes"

# all equal
assert run("""4 2
7 7 7 7
1 4
2 3
""") == "no\nno"

# no blockers
assert run("""5 1
1 2 3 4 5
1 5
""") == "yes"

# endpoint reversed order
assert run("""5 1
5 1 4 2 3
5 1
""") == "no"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | vâng | i = trường hợp j | 
| tất cả đều bình đẳng | không/không | quy tắc ngăn chặn bình đẳng | 
| mảng tăng dần | vâng | không có vật cản bên trong | 
| điểm cuối đảo ngược | không | chuẩn hóa i, j | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các điểm bên trong bằng điểm cực đại của điểm cuối. Coi như:```
4 1
5 5 5 5
1 4
```Đây$H = 5$. Vòng lặp kiểm tra chỉ số 2 trước, tìm$e_2 = 5 \ge 5$, và ngay lập tức trả về “không”. Điều này xác nhận rằng sự bình đẳng phải chặn khả năng hiển thị chứ không chỉ các giá trị lớn hơn. 

Một trường hợp khác là truy vấn một điểm:```
3 1
10 1 2
2 2
```Từ$i = j$, thuật toán sẽ bỏ qua toàn bộ quá trình quét và trả về "có", khớp với định nghĩa rằng một điểm luôn hiển thị với chính nó. 

Cuối cùng, điểm cuối đảo ngược:```
3 1
1 100 1
3 1
```Thuật toán hoán đổi chỉ số, tính toán$H = 1$và chỉ kiểm tra chỉ mục 2. Vì$100 \ge 1$, nó sẽ xuất ra chính xác “không”.
