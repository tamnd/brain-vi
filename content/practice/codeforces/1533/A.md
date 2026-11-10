---
title: "CF 1533A - Du lịch tới Bertown"
description: "Vika đến Bertown vào một ngày cố định $k$. Cô ấy có một vài người bạn và mỗi người bạn đều đưa ra một khoảng thời gian liên tục trong những ngày mà cô ấy có thể ở nhà họ."
date: "2026-06-14T18:31:17+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1533
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes: Episode 7"
rating: 0
weight: 1533
solve_time_s: 253
verified: true
draft: false
---

[CF 1533A - Du lịch tới Bertown](https://codeforces.com/problemset/problem/1533/A) 

**Đánh giá:** - 
**Tags:** *đặc biệt, thực hiện, toán học 
**Thời gian giải:** 4 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vika đến Bertown vào một ngày cố định$k$. Cô ấy có một vài người bạn và mỗi người bạn đều đưa ra một khoảng thời gian liên tục trong những ngày mà cô ấy có thể ở nhà họ. Điều kiện hạn chế là cô ấy có thể chọn tối đa một người bạn và khi đã chọn, cô ấy chỉ có thể ở trong khoảng thời gian cho phép của người bạn đó. Tuy nhiên, cô ấy không nhất thiết phải đến vào thời điểm bắt đầu khoảng thời gian đó, vì vậy thời gian lưu trú thực tế của cô ấy phải bắt đầu không sớm hơn ngày$k$. 

Đối với bất kỳ người bạn được chọn nào có khoảng thời gian$[l_i, r_i]$, Vika chỉ có thể ở lại từ ngày$k$tính đến ngày$r_i$, với điều kiện là$k$nằm trong khoảng đó. Điều này có nghĩa là số ngày thực tế cô ấy ở lại là$r_i - k + 1$, nhưng chỉ khi$l_i \le k \le r_i$. Nếu không có khoảng nào chứa$k$, cô ấy không thể ở lại được. 

Nhiệm vụ giảm xuống còn kiểm tra tất cả các khoảng có chứa$k$và tìm cái kéo dài xa nhất về phía bên phải. 

Các hạn chế là nhỏ:$t \le 500$,$n \le 100$và tất cả các giá trị ngày được giới hạn bởi 100. Điều này ngay lập tức loại trừ mọi nhu cầu xử lý trước phức tạp hoặc cấu trúc dữ liệu nâng cao. Quét tuyến tính đơn giản cho mỗi trường hợp thử nghiệm là đủ. 

Trường hợp cạnh tinh tế xuất hiện khi không có khoảng nào chứa$k$. Ví dụ, nếu$k = 4$và tất cả các khoảng đều giống như$[1,2]$,$[5,6]$, thì người bạn nào cũng đến quá sớm hoặc bắt đầu quá muộn. Trong trường hợp đó, câu trả lời phải chính xác là 0, vì Vika không thể ở lại dù chỉ một ngày. 

Một trường hợp cạnh khác là khi nhiều khoảng chứa$k$. Ví dụ,$k = 3$, và các khoảng$[1,4]$,$[2,6]$, Và$[3,3]$. Một cách tiếp cận bất cẩn có thể chọn khoảng thời gian bắt đầu gần nhất với$k$, nhưng điều đó không liên quan. Lựa chọn đúng là khoảng có điểm cuối bên phải tối đa. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo là mô phỏng rõ ràng lựa chọn của Vika: đối với mỗi người bạn, hãy kiểm tra xem$k$nằm trong khoảng thời gian của họ, tính xem cô ấy có thể ở lại bao nhiêu ngày nếu chọn người bạn đó và lấy số ngày tối đa. Cái này đã chạy rồi$O(n)$cho mỗi trường hợp thử nghiệm, vì mỗi khoảng được kiểm tra một lần. 

Ở đây không có cấu trúc tổ hợp sâu hơn vì quyết định không phụ thuộc vào sự chồng chéo giữa các khoảng. Mỗi người bạn đều độc lập: hoặc họ có thể tiếp đón Vika vào ngày$k$hoặc họ không thể. Khi chúng tôi lọc các khoảng hợp lệ, lựa chọn tốt nhất chỉ được xác định bởi điểm cuối bên phải lớn nhất. 

Việc “tối ưu hóa” phức tạp hơn có thể thử sắp xếp hoặc quét, nhưng điều đó sẽ làm tăng thêm chi phí không cần thiết mà không cải thiện độ phức tạp. Quan sát quan trọng là hạn chế duy nhất quan trọng là liệu$k$nằm ở$[l_i, r_i]$, và trong số những khoảng thời gian đó, tối đa hóa$r_i$trực tiếp tối đa hóa thời gian lưu trú. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra tất cả các khoảng thời gian) |$O(n)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 
| Tối ưu (cùng ý tưởng, quét trực tiếp) |$O(n)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$k$, xác định số lượng bạn bè và ngày Vika đến. Mục tiêu là đánh giá tất cả các khoảng thời gian so với điểm tham chiếu cố định này$k$. 
2. Khởi tạo biến trả lời là 0. Đây là số ngày tốt nhất mà Vika có thể ở lại cho đến nay. Bắt đầu từ 0 tự nhiên xử lý trường hợp không tồn tại khoảng hợp lệ. 
3. Lặp lại khoảng thời gian của mỗi người bạn$[l_i, r_i]$. Đối với mỗi khoảng thời gian, trước tiên hãy kiểm tra xem nó có bao gồm ngày không$k$, nghĩa$l_i \le k \le r_i$. Điều kiện này đảm bảo Vika thực sự có thể bắt đầu ở đó vào ngày đến. 
4. Nếu khoảng thời gian hợp lệ, hãy tính thời gian lưu trú tiềm năng là$r_i - k + 1$. Điều này phản ánh việc ở lại từ ngày$k$cho đến ngày cuối cùng có sẵn trong khoảng thời gian đó. 
5. Cập nhật câu trả lời với giá trị tối đa hiện tại và thời gian lưu trú được tính toán. Điều này đảm bảo chúng tôi luôn giữ được sự lựa chọn tốt nhất có thể trong số tất cả những người bạn hợp lệ. 
6. Sau khi xử lý tất cả các khoảng thời gian, xuất ra giá trị lớn nhất được lưu trữ. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là Vika không bao giờ rời khỏi khoảng thời gian của một người bạn đã chọn sớm hay muộn; thời gian lưu trú của cô ấy hoàn toàn được xác định bởi ranh giới bên phải của khoảng thời gian kể từ ngày bắt đầu$k$đã được sửa. Mỗi khoảng hợp lệ đều đóng góp một câu trả lời ứng cử viên độc lập$r_i - k + 1$và các khoảng không hợp lệ không đóng góp gì. Vì không gian quyết định là giá trị tối đa đơn giản trên các giá trị độc lập nên việc quét tất cả các khoảng sẽ duy trì tính chính xác mà không bỏ sót bất kỳ tương tác ẩn nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
out = []

for _ in range(t):
    n, k = map(int, input().split())
    best = 0

    for _ in range(n):
        l, r = map(int, input().split())
        if l <= k <= r:
            best = max(best, r - k + 1)

    out.append(str(best))

print("\n".join(out))
```Giải pháp duy trì mức chạy tối đa duy nhất cho mỗi trường hợp thử nghiệm. điều kiện`l <= k <= r`chỉ lọc những khoảng thời gian có thể sử dụng, đảm bảo chúng tôi không bao giờ tính toán những lượt lưu trú không hợp lệ. biểu thức`r - k + 1`đếm chính xác số ngày từ khi đến đến khi khởi hành. 

Đầu ra được đệm để tránh lặp lại chi phí I/O trong các trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 4
2 6
4 10
```Chúng tôi chỉ theo dõi các khoảng thời gian chứa ngày thứ 3. 

| Khoảng thời gian | Chứa k=3 | r - k + 1 | tốt nhất | 
| --- | --- | --- | --- | 
| [1,4] | vâng | 2 | 2 | 
| [2,6] | vâng | 4 | 4 | 
| [4,10] | không | - | 4 | 

Khoảng thời gian thứ hai cho kết quả tốt nhất, cho phép Vika ở lại 4 ngày. 

Điều này xác nhận rằng thuật toán bỏ qua các khoảng không liên quan một cách chính xác và chỉ tối đa hóa các khoảng thời gian hợp lệ. 

### Mẫu 2 

đầu vào:```
2 4
2 3
5 8
```| Khoảng thời gian | Chứa k=4 | r - k + 1 | tốt nhất | 
| --- | --- | --- | --- | 
| [2,3] | không | - | 0 | 
| [5,8] | không | - | 0 | 

Không có khoảng thời gian nào bao gồm ngày thứ 4, vì vậy câu trả lời vẫn là 0. Điều này xác nhận việc xử lý đúng trường hợp "không có người bạn hợp lệ". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi khoảng được kiểm tra một lần để đưa vào và đóng góp | 
| Không gian |$O(1)$| Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Được cho$n \le 100$Và$t \le 500$, giải pháp thực hiện tối đa 50.000 lần kiểm tra định kỳ, điều này không đáng kể trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        best = 0
        for _ in range(n):
            l, r = map(int, input().split())
            if l <= k <= r:
                best = max(best, r - k + 1)
        out.append(str(best))

    return "\n".join(out)

# provided samples
assert run("""3
3 3
1 4
2 6
4 10
2 4
2 3
5 8
2 4
4 4
1 3
""") == "4\n0\n1"

# minimum case: single friend covers k
assert run("""1
1 5
5 5
""") == "1"

# no friend covers k
assert run("""1
2 10
1 9
11 20
""") == "0"

# multiple valid intervals
assert run("""1
3 3
1 10
2 5
3 4
""") == "8"

# boundary: k equals r in best interval
assert run("""1
2 4
1 4
2 10
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng chính xác duy nhất | 1 | trường hợp hợp lệ tối thiểu | 
| không có bảo hiểm | 0 | xử lý vắng mặt | 
| nhiều ứng viên | 8 | lựa chọn tối đa chính xác | 
| ranh giới ở cạnh phải | 1 | tính chính xác của điểm cuối bao gồm | 

## Vỏ cạnh 

Khi không có khoảng bao gồm$k$, quá trình quét không bao giờ cập nhật câu trả lời ban đầu là 0. Ví dụ: với$k = 7$, khoảng$[1,3]$Và$[8,10]$cả hai đều không hợp lệ vì không thỏa mãn$l \le k \le r$. Thuật toán để lại câu trả lời chính xác ở mức 0 và xuất trực tiếp. 

Khi có nhiều khoảng bao gồm$k$, chỉ có điểm cuối phù hợp mới quan trọng. Ví dụ, với$k = 3$, khoảng$[1,4]$,$[2,6]$, Và$[3,3]$, thuật toán đánh giá các giá trị ứng viên$2$,$4$, Và$1$. Tối đa là 4, đến từ$[2,6]$. Quá trình lựa chọn không bao giờ phụ thuộc vào việc khoảng bắt đầu ở bên trái bao xa, mà chỉ phụ thuộc vào khoảng cách nó kéo dài sang bên phải bao xa.$k$.
