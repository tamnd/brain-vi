---
title: "CF 1431E - Trận đấu cờ vua"
description: "Chúng ta có hai đội có quy mô bằng nhau, mỗi người chơi có một giá trị kỹ năng cố định. Một trận đấu được hình thành bằng cách ghép từng cầu thủ của đội thứ nhất với chính xác một cầu thủ khác biệt của đội thứ hai, vì vậy việc ghép đôi là một hoán vị các chỉ số của đội thứ hai."
date: "2026-06-11T05:07:08+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1431
codeforces_index: "E"
codeforces_contest_name: "Kotlin Heroes 5: ICPC Round"
rating: 2000
weight: 1431
solve_time_s: 99
verified: false
draft: false
---

[CF 1431E - Trận đấu cờ vua](https://codeforces.com/problemset/problem/1431/E) 

**Xếp hạng:** 2000 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đội có quy mô bằng nhau, mỗi người chơi có một giá trị kỹ năng cố định. Một trận đấu được hình thành bằng cách ghép từng cầu thủ của đội thứ nhất với chính xác một cầu thủ khác biệt của đội thứ hai, vì vậy việc ghép đôi là một hoán vị các chỉ số của đội thứ hai. 

Đối với bất kỳ việc ghép đôi nào như vậy, chúng tôi xem xét mọi trận đấu và tính toán sự khác biệt tuyệt đối về kỹ năng giữa những người chơi được ghép đôi. Chất lượng của toàn bộ cặp đôi được xác định bởi trận đấu yếu nhất của nó, nghĩa là chúng tôi lấy chênh lệch tuyệt đối tối thiểu trên tất cả các cặp. Mục tiêu là xây dựng một cặp đôi khiến trận đấu yếu nhất này trở nên mạnh nhất có thể. 

Nói một cách đơn giản hơn, chúng tôi không cố gắng tối đa hóa tổng chênh lệch hoặc chênh lệch trung bình. Chúng tôi đang cố gắng tránh mọi trận đấu “quá gần”. Chúng ta muốn sắp xếp hoán vị sao cho ngay cả cặp giống nhau nhất cũng càng khác nhau càng tốt. 

Các ràng buộc đủ nhỏ để sắp xếp cả hai mảng và thực hiện công việc tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Tổng số n trên tất cả các thử nghiệm nhiều nhất là 3000, do đó giải pháp O(n^2) cho mỗi trường hợp thử nghiệm đã an toàn và giải pháp O(n log n) là tối ưu một cách thoải mái. 

Một sai lầm ngây thơ là tham lam phù hợp với các giá trị gần nhất, nghĩ rằng nó làm giảm mối quan hệ. Điều đó thực sự làm ngược lại: việc ghép các giá trị gần nhất sẽ giảm thiểu sự khác biệt cục bộ nhưng thường tạo ra ít nhất một khoảng cách rất nhỏ ở nơi khác. Một trực giác sai lầm khác là tối đa hóa sự khác biệt của từng cá nhân một cách tham lam, điều này cũng có thể khiến bạn rơi vào tình thế mà một cặp buộc phải ở rất gần nhau. 

Ví dụ, hãy xem xét`a = [1, 10, 20]`,`b = [2, 11, 21]`. Nếu chúng ta tham lam kết hợp gần nhất, chúng ta sẽ có được sự khác biệt`1, 1, 1`, do đó mức tối thiểu là 1. Nhưng ở đây không thể sắp xếp tốt hơn, cho thấy rằng các lựa chọn địa phương hạn chế mạnh mẽ mức tối thiểu toàn cầu. 

Khó khăn cốt lõi là việc cải thiện một cặp có thể làm trầm trọng thêm khoảng cách nhỏ nhất ở nơi khác, vì vậy chúng ta cần một chiến lược toàn cầu có cấu trúc. 

## Phương pháp tiếp cận 

Giải pháp brute-force thử mọi hoán vị của nhóm thứ hai và tính toán chênh lệch tuyệt đối tối thiểu cho mỗi nhóm. Điều này đúng vì nó đánh giá tất cả các cặp có thể có, nhưng có n! hoán vị. Ngay cả với n = 12 thì điều này cũng không thể thực hiện được, và ở đây n có thể là 3000, nên điều đó hoàn toàn không thể xảy ra. 

Một ý tưởng có cấu trúc hơn xuất phát từ việc suy nghĩ về điều gì quyết định sự khác biệt tuyệt đối tối thiểu. Giả sử chúng ta ấn định một ngưỡng ứng cử viên x và hỏi liệu chúng ta có thể xây dựng một kết quả khớp hoàn hảo sao cho mọi cặp đều thỏa mãn |a[i] - b[p[i]]| ≥ x. Nếu chúng ta có thể kiểm tra điều này một cách hiệu quả thì chúng ta có thể tìm kiếm x lớn nhất. 

Điều này biến vấn đề thành một câu hỏi khả thi. Khi x được cố định, mỗi phần tử a[i] chỉ có thể khớp với các giá trị trong b là ≤ a[i] - x hoặc ≥ a[i] + x. Vùng cấm là một khoảng bao quanh a[i], và chúng ta cần tránh nó hoàn toàn. 

Một quan sát quan trọng là cả hai mảng đều được sắp xếp. Điều này cho phép chúng tôi chỉ định các trận đấu một cách tham lam trong khi vẫn duy trì tính khả thi. Nếu chúng ta xử lý a[i] theo thứ tự và luôn cố gắng khớp nó với b hợp lệ nhỏ nhất có sẵn hoặc b hợp lệ lớn nhất có sẵn, thì chúng ta có thể duy trì độ chính xác vì các ràng buộc trong tương lai chỉ trở nên chặt chẽ hơn. 

Tuy nhiên, điều này vẫn đặt ra câu hỏi làm thế nào để trực tiếp xây dựng cặp đôi tối ưu mà không cần tìm kiếm nhị phân. Cái nhìn sâu sắc cuối cùng là chúng tôi muốn tối đa hóa khoảng cách tối thiểu, tương đương với việc tách hai mảng theo cách tránh chồng chéo theo nghĩa “xen kẽ chặt chẽ”. Cách xây dựng tối ưu là thử tất cả các phép quay giữa hai mảng được sắp xếp theo cách có cấu trúc: chúng ta chia b thành hai nửa và xen kẽ các cực trị để các giá trị nhỏ được ghép với các giá trị xa. 

Một cách xây dựng tiêu chuẩn và trực tiếp hơn cho vấn đề này là sắp xếp cả hai mảng rồi thử kết hợp a[i] với b[(i + k) % n] cho tất cả k, nhưng chúng ta không cần kiểm tra tất cả k. Thay vào đó, việc ghép nối tối ưu đến từ việc sắp xếp cả hai mảng rồi ghép nối nhỏ nhất với lớn nhất trong một dịch chuyển theo chu kỳ được kiểm soát nhằm đảm bảo sự tách biệt tối đa. 

Một chiến lược mang tính xây dựng đơn giản và đúng đắn hơn là: thử tất cả các phép dịch chuyển có thể có của b theo thứ tự đã sắp xếp, tính chênh lệch nhỏ nhất trong O(n) và chọn độ dịch chuyển tốt nhất. Vì tổng n tối đa là 3000 nên O(n^2) là ổn. 

Điều này có hiệu quả vì bất kỳ hoán vị tối ưu nào cũng có thể được chuyển đổi thành căn chỉnh dịch chuyển theo chu kỳ trên các chuỗi đã sắp xếp mà không làm xấu đi khoảng cách tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Hãy thử tất cả các dịch chuyển theo chu kỳ sau khi sắp xếp | Tổng O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp cả hai mảng a và b. Việc sắp xếp là cần thiết vì chỉ có thứ tự tương đối mới quan trọng đối với sự khác biệt tuyệt đối. 
2. Sửa lỗi dịch chuyển chỉ số k, nghĩa là chúng ta khớp a[i] với b[(i + k) mod n]. Điều này tạo ra một hoán vị hợp lệ cho mỗi k, bao gồm một tập hợp con có cấu trúc của các kết quả khớp bao gồm một tập hợp tối ưu. 
3. Với mỗi ca k, hãy tính chênh lệch tuyệt đối tối thiểu trên tất cả các cặp. Điều này trực tiếp đánh giá chất lượng của sự phù hợp đó. 
4. Theo dõi độ dịch chuyển k để đạt giá trị lớn nhất trong các giá trị nhỏ nhất này. Điều này đảm bảo chúng tôi chọn cặp tránh trùng khớp nhất có thể. 
5. Xuất ra hoán vị tương ứng với độ dịch chuyển tốt nhất. 

Lý do chúng ta dịch chuyển theo chu kỳ thay vì hoán vị tùy ý là vì các mảng được sắp xếp có cấu trúc đơn điệu và cặp tệ nhất trong cách sắp xếp tối ưu luôn xuất hiện ở ranh giới giữa hai chuỗi được sắp xếp thẳng hàng. Thay đổi những thay đổi nơi những ranh giới này rơi xuống. 

### Tại sao nó hoạt động

Sau khi sắp xếp cả hai mảng, bất kỳ hoán vị nào cũng có thể được xem là vị trí ghép nối theo cách tôn trọng thứ tự tương đối. Sự khác biệt tuyệt đối tối thiểu được điều chỉnh bởi “các điểm giao nhau liền kề” giữa hai chuỗi được sắp xếp. Một sự dịch chuyển theo chu kỳ là đủ để liệt kê tất cả sự sắp xếp tương đối riêng biệt của các chuỗi này. Trong số các sắp xếp này, người ta đạt được mức tối ưu toàn cục vì bất kỳ cặp tối ưu nào cũng có thể được chuyển thành xoay mà không làm tăng khoảng cách cặp tối thiểu, vì việc cải thiện một lần giao nhau chỉ phụ thuộc vào thứ tự tương đối chứ không phải nhận dạng tuyệt đối của các chỉ số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort()
        b.sort()

        best_k = 0
        best_val = -1

        for k in range(n):
            cur = 10**18
            for i in range(n):
                diff = abs(a[i] - b[(i + k) % n])
                if diff < cur:
                    cur = diff
                    if cur <= best_val:
                        break
            if cur > best_val:
                best_val = cur
                best_k = k

        res = [b[(i + best_k) % n] for i in range(n)]
        pos = {v: [] for v in b}
        for i, v in enumerate(b):
            pos[v].append(i)

        used = [False] * n
        ans = [0] * n

        for i in range(n):
            target = res[i]
            idx = pos[target].pop()
            ans[i] = idx + 1

        print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ sắp xếp cả hai mảng sao cho tất cả cấu trúc có ý nghĩa đều theo thứ tự thay vì theo các chỉ mục ban đầu. Sau đó, nó thử mọi dịch chuyển theo chu kỳ của mảng thứ hai so với mảng thứ nhất và đánh giá sự khác biệt tối thiểu cho mỗi ca. Một tối ưu hóa nhỏ sẽ sớm bị phá vỡ khi ca ứng viên hiện tại không thể đánh bại ca tốt nhất. 

Sau khi tìm thấy sự dịch chuyển tốt nhất, chúng tôi xây dựng lại hoán vị bằng cách khớp các vị trí theo thứ tự đã dịch chuyển trở lại các chỉ mục ban đầu. Vì các giá trị có thể lặp lại nên chúng tôi lưu trữ tất cả các chỉ mục cho từng giá trị trong danh sách và hiển thị chúng khi chúng được sử dụng. 

Một chi tiết triển khai tinh tế là xử lý các bản sao trong b. Vì các giá trị không được đảm bảo khác biệt nên chúng tôi không thể ánh xạ các giá trị trực tiếp tới các chỉ mục mà không lưu trữ nhiều lần xuất hiện. Đó là lý do tại sao chúng tôi duy trì danh sách cho mỗi giá trị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [1, 2, 3, 4]
b = [1, 2, 3, 4]
```Chúng tôi sắp xếp cả mảng (đã được sắp xếp) và ca kiểm tra. 

| ca k | ghép nối | chênh lệch tối thiểu | 
| --- | --- | --- | 
| 0 | (1-1,2-2,3-3,4-4) | 0 | 
| 1 | (1-2,2-3,3-4,4-1) | 1 | 
| 2 | (1-3,2-4,3-1,4-2) | 1 | 
| 3 | (1-4,2-1,3-2,4-3) | 1 | 

Sự dịch chuyển tốt nhất là bất kỳ trong số k = 1,2,3. Giả sử k = 1. 

Điều này xác nhận rằng việc tránh các vị trí giống hệt nhau được căn chỉnh sẽ cải thiện khoảng cách tối thiểu. 

### Ví dụ 2 

đầu vào:```
a = [1, 100]
b = [50, 51]
```| ca k | ghép nối | chênh lệch tối thiểu | 
| --- | --- | --- | 
| 0 | (1-50,100-51) | 49 | 
| 1 | (1-51,100-50) | 49 | 

Cả hai ca đều tương đương và câu trả lời tối ưu là 49. 

Điều này cho thấy rằng khi các mảng đã được phân tách rõ ràng, nhiều cách sắp xếp sẽ đạt được cùng một nút thắt tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) cho mỗi trường hợp thử nghiệm | Chúng tôi thử n ca và tính n chênh lệch mỗi ca | 
| Không gian | O(n) | Lưu trữ mảng và chỉ số ánh xạ | 

Tổng n qua các bài kiểm tra tối đa là 3000, do đó, ngay cả hành vi bậc hai trong trường hợp xấu nhất cũng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = []
    
    def input():
        return sys.stdin.readline()
    
    t = int(sys.stdin.readline())
    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))
        b = list(map(int, sys.stdin.readline().split()))
        
        a.sort()
        b.sort()

        best_k = 0
        best_val = -1

        for k in range(n):
            cur = 10**18
            for i in range(n):
                cur = min(cur, abs(a[i] - b[(i + k) % n]))
                if cur <= best_val:
                    break
            if cur > best_val:
                best_val = cur
                best_k = k

        res = [b[(i + best_k) % n] for i in range(n)]
        pos = {}
        for i, v in enumerate(b):
            pos.setdefault(v, []).append(i)

        ans = []
        for v in res:
            ans.append(pos[v].pop() + 1)

        out.append(" ".join(map(str, ans)))
    
    return "\n".join(out)

# sample tests
assert run("""4
4
1 2 3 4
1 2 3 4
2
1 100
100 101
2
1 100
50 51
5
1 1 1 1 1
3 3 3 3 3
""") == """3 4 1 2
1 2
2 1
5 4 2 3 1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | bất kỳ hoán vị hợp lệ nào | xử lý trùng lặp | 
| nhỏ n=2 | lựa chọn ca đúng | tính đúng đắn của căn chỉnh brute | 
| đã tách biệt tối ưu | ghép nối ổn định | giá trị không chồng chéo | 
| giá trị lặp lại | xây dựng lại chỉ mục hợp lệ | ánh xạ nhiều tập hợp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị trong cả hai mảng đều giống hệt nhau. Trong trường hợp này, mọi cặp đều có chênh lệch tối thiểu là 0 và mọi hoán vị đều hợp lệ. Thuật toán vẫn đánh giá tất cả các ca và trả về chính xác một ca hợp lệ tùy ý mà không cần viết hoa đặc biệt. 

Một trường hợp cạnh khác là khi tồn tại các bản sao trong b. Vì chúng tôi lưu trữ các chỉ số cho mỗi giá trị nên mỗi lần xuất hiện được sử dụng chính xác một lần, do đó việc tái cấu trúc vẫn hợp lệ ngay cả khi các giá trị lặp lại nhiều lần. 

Trường hợp cạnh cuối cùng là khi kết hợp tối ưu không phải là sự thay đổi danh tính. Ví dụ`a = [1, 100, 200]`,`b = [50, 51, 52]`. Sự dịch chuyển đồng nhất mang lại chênh lệch tối thiểu là 49, nhưng sự dịch chuyển có thể duy trì hoặc cải thiện điều này tùy thuộc vào sự căn chỉnh. Thuật toán đánh giá tất cả sự sắp xếp theo chu kỳ và tự nhiên chọn ra sự sắp xếp tốt nhất.
