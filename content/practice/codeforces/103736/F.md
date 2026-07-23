---
title: "CF 103736F - Mảng con"
description: "Chúng ta được cho một dãy số nguyên và được yêu cầu đếm xem có bao nhiêu mảng con liền kề có tổng chia hết cho một số nguyên $k$ đã cho."
date: "2026-07-02T09:11:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103736
codeforces_index: "F"
codeforces_contest_name: "The 2022 Hangzhou Normal U Summer Trials"
rating: 0
weight: 103736
solve_time_s: 47
verified: true
draft: false
---

[CF 103736F - Mảng con](https://codeforces.com/problemset/problem/103736/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và được yêu cầu đếm xem có bao nhiêu mảng con liền kề có tổng chia hết cho một số nguyên đã cho$k$. Mảng con ở đây có nghĩa là chúng ta chọn vị trí bắt đầu và vị trí kết thúc rồi lấy mọi thứ ở giữa mà không có khoảng trống, sau đó tính tổng của các phần tử đó. Chúng tôi muốn biết có bao nhiêu số tiền này là bội số của$k$, bao gồm cả số không. 

Độ dài mảng có thể lên tới$10^5$và mỗi giá trị có thể lớn bằng$10^9$. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng tính lại tổng từ đầu cho mỗi mảng con, vì bản thân số lượng mảng con là khoảng$O(n^2)$, nó quá lớn. Ngay cả một vòng lặp bậc hai cũng đã có sẵn$10^{10}$hoạt động trong trường hợp xấu nhất, không thể vượt qua giới hạn 1 giây. 

Một điểm tinh tế là các giá trị có thể bằng 0, do đó, một mảng con bao gồm một số 0 phải được tính khi$k$chia số không. Một trường hợp góc khác xuất hiện khi$k = 1$, trong đó mọi mảng con đều hợp lệ vì mọi số nguyên đều chia hết cho 1, nghĩa là câu trả lời phải là$n(n+1)/2$. Bất kỳ giải pháp chính xác nào cũng phải xử lý việc này một cách tự nhiên mà không cần vỏ bọc đặc biệt. 

Kiểm tra tổng tiền tố đơn giản cũng có thể trông an toàn nhưng vẫn không thành công do lo ngại tràn trong các ngôn ngữ có số nguyên có chiều rộng cố định, vì tổng tiền tố có thể đạt tới$10^{14}$. Trong Python đây không phải là một vấn đề, nhưng trong các bối cảnh khác, nó quan trọng về mặt khái niệm. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi cặp$(l, r)$, tính tổng của mảng con$a_l \dots a_r$và kiểm tra xem nó có chia hết cho không$k$. Ngay cả khi chúng tôi tính toán trước tổng tiền tố, chúng tôi vẫn lặp lại tất cả các cặp chỉ số, dẫn đến$O(n^2)$séc. 

Lý do điều này thất bại không chỉ là số lượng mảng con mà còn do mỗi cặp có giá$O(1)$với tổng tiền tố hoặc$O(n)$không có họ. Dù bằng cách nào, tỷ lệ bậc hai là không thể tránh khỏi. 

Quan sát quan trọng là tổng của mảng con trở nên đơn giản khi được biểu thị thông qua tổng tiền tố. Cho phép$S_i = a_1 + \dots + a_i$. Khi đó tổng của một mảng con$[l, r]$là$S_r - S_{l-1}$. Chúng tôi muốn cái này được chia cho$k$, nghĩa:$$S_r \equiv S_{l-1} \pmod{k}$$Vì vậy, vấn đề không còn là về mảng con nữa mà là về việc đếm các cặp tổng tiền tố có phần dư bằng modulo$k$. Mỗi mảng con hợp lệ tương ứng chính xác với một cặp chỉ số có tổng tiền tố có cùng lớp modulo. 

Điều này biến bài toán thành các tổ hợp đếm trong nhóm: với mỗi giá trị còn lại$r$, nếu nó xuất hiện$c_r$lần trong số các tổng tiền tố, nó đóng góp$c_r(c_r - 1)/2$mảng con hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) hoặc O(n) | Quá chậm | 
| Tối ưu (đếm mod tiền tố) | O(n) | O(phút(n, k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố trong khi lặp qua mảng, duy trì tổng modulo$k$thay vì toàn bộ số tiền. Điều này tránh sự tăng trưởng số nguyên lớn và chỉ giữ lại lớp tương đương có liên quan. 
2. Duy trì một bản đồ tần suất ghi lại số lần mỗi phần còn lại đã xuất hiện cho đến nay. Khởi tạo nó với số dư 0 xuất hiện một lần, biểu thị tiền tố trống trước khi bất kỳ phần tử nào được lấy. 
3. Đối với mỗi phần tử, cập nhật tổng tiền tố đang chạy và tính modulo còn lại của nó$k$. 
4. Mỗi lần chúng ta thấy phần còn lại$r$, chúng ta biết rằng tất cả các tổng tiền tố trước đó có cùng số dư có thể tạo thành một mảng con hợp lệ kết thúc ở vị trí hiện tại. Vì vậy, chúng tôi thêm tần số hiện tại của$r$để trả lời. 
5. Tăng tần suất còn lại$r$. 

Quyết định quan trọng là bước 4. Thay vì cố gắng hình thành các mảng con một cách rõ ràng, chúng tôi diễn giải mỗi tổng tiền tố là một “trạng thái” và mỗi lần lặp lại một trạng thái sẽ tạo ra các mảng con hợp lệ mới kết thúc ở chỉ mục hiện tại. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ tính bất biến ở bất kỳ vị trí nào$i$, bản đồ tần số lưu trữ chính xác có bao nhiêu tổng tiền tố có mỗi phần còn lại giữa các chỉ số$0 \dots i$. Một mảng con$[l, r]$có tổng chia hết cho$k$khi và chỉ nếu tiền tố còn lại tại$r$Và$l-1$đều bình đẳng. Vì vậy mỗi khi chúng ta gặp phần còn lại$r$, chúng tôi đang chọn một cách hiệu quả bất kỳ lần xuất hiện trước đó của cùng phần dư làm ranh giới bắt đầu của một mảng con hợp lệ kết thúc ở chỉ mục hiện tại. Không có cặp hợp lệ nào bị bỏ sót và không có cặp không hợp lệ nào được tính vì các số dư khác nhau không thể thỏa mãn điều kiện chia hết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
arr = list(map(int, input().split()))

freq = {0: 1}
prefix = 0
ans = 0

for x in arr:
    prefix = (prefix + x) % k
    ans += freq.get(prefix, 0)
    freq[prefix] = freq.get(prefix, 0) + 1

print(ans)
```Việc triển khai theo dõi modulo tổng tiền tố$k$trực tiếp, điều này tránh được cả tình trạng tràn và tính toán tổng không cần thiết. Từ điển`freq`lưu trữ số lượng của mỗi phần còn lại. Mục nhập ban đầu`{0: 1}`là cần thiết vì nó tính đến các mảng con bắt đầu từ chỉ mục 1, trong đó tiền tố đã chia hết cho$k$. 

Một cạm bẫy phổ biến là cập nhật tần suất trước khi thêm vào câu trả lời. Làm như vậy sẽ đếm không chính xác tiền tố hiện tại với chính nó, tiền tố này không tương ứng với bất kỳ mảng con hợp lệ nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 3
0 1 2 4 7 7
```Chúng tôi theo dõi tiền tố modulo 3 và cập nhật tần số. 

| tôi | giá trị | tiền tố mod 3 | tần số trước | đã thêm vào câu trả lời | tần số sau | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | {0:1} | 1 | {0:2} | 1 | 
| 2 | 1 | 1 | {0:2} | 0 | {0:2,1:1} | 1 | 
| 3 | 2 | 0 | {0:2,1:1} | 2 | {0:3,1:1} | 3 | 
| 4 | 4 | 2 | {0:3,1:1} | 0 | {0:3,1:1,2:1} | 3 | 
| 5 | 7 | 0 | {0:3,1:1,2:1} | 3 | {0:4,1:1,2:1} | 6 | 
| 6 | 7 | 1 | {0:4,1:1,2:1} | 1 | {0:4,1:2,2:1} | 7 | 

Dấu vết này cho thấy phần còn lại của tiền tố lặp lại trực tiếp tạo ra nhiều mảng con hợp lệ như thế nào mà không liệt kê chúng một cách rõ ràng. 

### Ví dụ 2 

đầu vào:```
5 2
1 1 1 1 1
```| tôi | giá trị | tiền tố mod 2 | tần số trước | đã thêm vào câu trả lời | tần số sau | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | {0:1} | 0 | {0:1,1:1} | 0 | 
| 2 | 1 | 0 | {0:1,1:1} | 1 | {0:2,1:1} | 1 | 
| 3 | 1 | 1 | {0:2,1:1} | 1 | {0:2,1:2} | 2 | 
| 4 | 1 | 0 | {0:2,1:2} | 2 | {0:3,1:2} | 4 | 
| 5 | 1 | 1 | {0:3,1:2} | 2 | {0:3,1:3} | 6 | 

Điều này thể hiện cấu trúc xen kẽ: mỗi cặp số dư bằng nhau tạo thành một mảng con hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Trung bình mỗi phần tử được xử lý một lần với các phép toán từ điển O(1) | 
| Không gian | O(phút(n, k)) | Chúng tôi lưu trữ tần số của số dư gặp phải | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$n \le 10^5$, đồng thời cả thời gian và mức sử dụng bộ nhớ đều tăng tuyến tính với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    freq = {0: 1}
    prefix = 0
    ans = 0

    for x in arr:
        prefix = (prefix + x) % k
        ans += freq.get(prefix, 0)
        freq[prefix] = freq.get(prefix, 0) + 1

    return str(ans)

# provided sample
assert run("6 3\n0 1 2 4 7 7\n") == "7"

# single element zero
assert run("1 3\n0\n") == "1"

# all ones, k = 2
assert run("5 2\n1 1 1 1 1\n") == "6"

# k = 1, all subarrays valid
assert run("4 1\n1 2 3 4\n") == "10"

# alternating zeros
assert run("5 3\n0 0 0 0 0\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 3 … | 7 | độ chính xác của mẫu | 
| 1 3 / 0 | 1 | trường hợp không cạnh đơn | 
| k=2 cái | 6 | cấu trúc tiền tố lặp đi lặp lại | 
| k=1 | 10 | tổ hợp đầy đủ | 
| tất cả số không | 15 | tất cả các tiền tố bằng nhau | 

## Vỏ cạnh 

Trường hợp tối thiểu như một phần tử đơn sẽ kiểm tra xem phần tiền tố còn lại ban đầu có được xử lý chính xác hay không. Với đầu vào`1 3`và mảng`[0]`, phần còn lại của tiền tố bằng 0 ngay lập tức và vì bản đồ tần số bắt đầu bằng`{0:1}`, chúng ta đếm chính xác một mảng con hợp lệ. Thuật toán xử lý việc này như một bước duy nhất trong đó`prefix = 0`, thêm`freq[0] = 1`, và trả về 1. 

Một trường hợp với`k = 1`cực đoan hơn vì mọi tiền tố đều có số dư bằng 0. Vì`n = 4`Và`[1,2,3,4]`, mỗi bước sẽ tăng tần số của số 0 còn lại, tạo ra tổng của tất cả các mảng con là 10. Cơ chế tần số tích lũy số lượng tam giác một cách tự nhiên mà không có bất kỳ sửa đổi nào. 

Một mảng hoàn toàn bằng 0 hoạt động tương tự nhưng với bất kỳ`k`. Vì mọi tổng tiền tố đều giống hệt modulo`k`, mỗi cặp chỉ số tạo thành một mảng con hợp lệ. Bản đồ tần số liên tục đếm các kết hợp tăng dần, tạo ra$n(n+1)/2$, phù hợp với cấu trúc tổ hợp dự kiến.
