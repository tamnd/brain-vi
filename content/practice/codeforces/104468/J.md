---
title: "CF 104468J - Mảng tiện ích Elias"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi cái có một mảng các số nguyên và chúng ta được phép chọn một tập hợp con của các giá trị này. Tập hợp con được coi là hợp lệ nếu mọi cặp phần tử được chọn đều thỏa mãn bất đẳng thức bitwise cụ thể liên quan đến AND và XOR."
date: "2026-06-30T13:00:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104468
codeforces_index: "J"
codeforces_contest_name: "The 2023 Damascus University Collegiate Programming Contest"
rating: 0
weight: 104468
solve_time_s: 106
verified: false
draft: false
---

[CF 104468J - Mảng Elias-utiful](https://codeforces.com/problemset/problem/104468/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi cái có một mảng các số nguyên và chúng ta được phép chọn một tập hợp con của các giá trị này. Tập hợp con được coi là hợp lệ nếu mọi cặp phần tử được chọn đều thỏa mãn bất đẳng thức bitwise cụ thể liên quan đến AND và XOR. 

Đối với bất kỳ hai số được chọn nào, chúng tôi so sánh giá trị AND theo bit của chúng với XOR theo bit của chúng dưới dạng số nguyên thông thường. Tập hợp con chỉ được chấp nhận nếu, đối với mỗi cặp, kết quả AND lớn hơn hoặc bằng kết quả XOR. Nhiệm vụ là chọn tập con lớn nhất có thể thỏa mãn điều kiện này và xuất ra kích thước của nó. 

Ràng buộc về tổng kích thước đầu vào trong các trường hợp thử nghiệm là lớn, tổng thể lên tới 10^5 số. Điều này ngay lập tức loại trừ việc kiểm tra tất cả các cặp bên trong mỗi trường hợp thử nghiệm, vì phương pháp bậc hai sẽ yêu cầu tối đa khoảng 10^10 phép so sánh trong trường hợp xấu nhất, vượt xa mức cho phép trong 1 giây. Bất kỳ giải pháp hợp lệ nào cũng phải giảm vấn đề xuống gần tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện với số không. Nếu số 0 được ghép với bất kỳ số dương nào, XOR bằng số dương trong khi AND bằng 0, vi phạm điều kiện. Điều này có nghĩa là số 0 chỉ có thể cùng tồn tại với những số 0 khác. 

Một tình huống không hề tầm thường khác là khi các số trông có vẻ “gần giống” về giá trị nhưng khác nhau ở các bit cao hơn. Ví dụ: 5 (101), 6 (110) và 7 (111) tạo thành một nhóm hợp lệ có kích thước 3, mặc dù các cặp như 5 và 6 khác nhau ở nhiều bit. Một phương pháp phỏng đoán ngây thơ như “các con số phải giống nhau” là không đủ chính xác; tính chính xác phụ thuộc hoàn toàn vào cấu trúc của bit được đặt cao nhất của chúng. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: thử mọi tập hợp con và xác minh điều kiện cho mọi cặp bên trong nó. Ngay cả một phiên bản ít cực đoan hơn một chút, trong đó chúng tôi sắp xếp các tập hợp con và kiểm tra tính hợp lệ tăng dần, cuối cùng vẫn phải kiểm tra các cặp liên tục. Đối với một tập hợp con có kích thước k, việc xác thực nó có giá O(k^2) và vì có nhiều tập hợp con theo cấp số nhân nên cách tiếp cận này sẽ thất bại ngay lập tức đối với N lên tới 10^5. 

Quan sát quan trọng đến từ việc phân tích khi nào thì bất đẳng thức có thể sai. Xét hai số a và b. Ở bit quan trọng nhất mà chúng khác nhau, XOR có 1 trong khi AND có 0. Bit đơn đó chiếm ưu thế so sánh vì nó đóng góp giá trị 2^k cho XOR, trong khi AND không thể đóng góp bất cứ điều gì ở vị trí đó. Nếu AND chưa chứa số hạng 2^k thì các bit thấp hơn không thể bù được. Điều này có nghĩa là bất cứ khi nào hai số có bit được đặt cao nhất khác nhau thì điều kiện sẽ không thành công. 

Bây giờ hãy xem điều gì sẽ xảy ra nếu hai số có cùng bit được đặt cao nhất. Cả hai đều có tập hợp bit đó, vì vậy AND cũng có tập hợp bit đó, đóng góp ít nhất 2^k. XOR hoàn toàn không thể đặt bit đó, vì vậy giá trị của nó hoàn toàn nhỏ hơn 2^k. Điều này đảm bảo sự bất đẳng thức đúng cho mọi cặp trong một nhóm như vậy. 

Điều này làm giảm vấn đề nhóm các số theo vị trí của bit thiết lập quan trọng nhất của chúng và chọn nhóm lớn nhất. Số 0 phải được xử lý riêng vì nó không có bit được thiết lập và không thành công đối với mọi số dương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N · N^2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng cốt lõi: nhóm theo bit quan trọng nhất 

## Hướng dẫn thuật toán

1. Đối với mỗi số, xác định vị trí của bit được đặt cao nhất. Đây là tính năng duy nhất quan trọng đối với khả năng tương thích vì nó xác định thang đo vượt trội của con số. 
2. Duy trì bộ đếm tần số cho từng vị trí bit có thể từ 0 đến 29, vì các giá trị được giới hạn bởi 2^30. 
3. Xử lý số 0 một cách riêng biệt bằng cách đếm xem có bao nhiêu số 0 tồn tại, vì nó không thuộc bất kỳ nhóm bit nào và không thể trộn lẫn với các giá trị khác 0. 
4. Đối với mọi số khác 0, hãy tăng bộ đếm của bit quan trọng nhất của nó. 
5. Câu trả lời cho một trường hợp kiểm thử là giá trị lớn nhất trong số tất cả các bộ đếm bit và bộ đếm 0. 

Lý do lựa chọn tham lam này đúng là vì bất kỳ tập hợp con hợp lệ nào cũng phải có tất cả các phần tử có chung bit được đặt cao nhất, nếu không thì ít nhất một cặp sẽ vi phạm bất đẳng thức. Khi hạn chế đó được thực thi, tất cả các thành phần trong cùng một nhóm đều tương thích lẫn nhau, do đó việc lấy toàn bộ nhóm luôn là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        arr = list(map(int, input().split()))

        cnt = [0] * 30
        zero = 0

        for x in arr:
            if x == 0:
                zero += 1
                continue

            msb = x.bit_length() - 1
            cnt[msb] += 1

        print(max(max(cnt), zero))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên Python`bit_length()`để tính bit được đặt cao nhất trong thời gian O(1) cho mỗi số. Mảng`cnt`lưu trữ bao nhiêu số rơi vào mỗi nhóm bit. Sự riêng biệt`zero`bộ đếm đảm bảo tính chính xác cho trường hợp đặc biệt trong đó tất cả các phần tử được chọn có thể bằng 0. 

Câu trả lời cuối cùng là tần suất tối đa trên tất cả các nhóm vì bất kỳ tập hợp con hợp lệ nào cũng phải được chứa hoàn toàn trong một nhóm như vậy và việc lấy tất cả các phần tử của nhóm đó luôn an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[6, 8, 2, 7, 2, 5]`| Giá trị | Nhị phân | MSB | Số nhóm | 
| --- | --- | --- | --- | 
| 6 | 110 | 2 | 1 | 
| 8 | 1000 | 3 | 1 | 
| 2 | 010 | 1 | 2 | 
| 7 | 111 | 2 | 2 | 
| 5 | 101 | 2 | 3 | 

Ở đây, nhóm lớn nhất là MSB = 2 với ba phần tử: 6, 7 và 5. Tất cả những phần tử này có thể cùng tồn tại vì chúng có chung bit cao nhất và mọi cặp đều thỏa mãn bất đẳng thức. 

### Ví dụ 2 

Mảng đầu vào:`[0, 0, 1, 2]`| Giá trị | Nhóm | 
| --- | --- | 
| 0 | nhóm không | 
| 1 | MSB 0 | 
| 2 | MSB 1 | 

Nhóm 0 có kích thước 2, trong khi các nhóm khác có kích thước 1. Vì số 0 không thể trộn lẫn với các giá trị khác 0 nên tập hợp con tối ưu là hai số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được xử lý một lần để tính MSB của nó và cập nhật bộ đếm | 
| Không gian | O(1) | Chỉ sử dụng một mảng cố định có kích thước 30 bất kể kích thước đầu vào | 

Giải pháp phù hợp một cách thoải mái trong các ràng buộc vì tổng số thao tác là tuyến tính trên tất cả các trường hợp thử nghiệm cộng lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log2

    input = sys.stdin.readline
    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        arr = list(map(int, input().split()))
        cnt = [0] * 30
        zero = 0
        for x in arr:
            if x == 0:
                zero += 1
            else:
                cnt[x.bit_length() - 1] += 1
        out.append(str(max(max(cnt), zero)))
    return "\n".join(out)

# provided sample (format interpreted)
assert run("""3
6
8 6 2 7 2 5
1
3
2
1 0
""") == """3
1
1"""

# all zeros
assert run("""1
5
0 0 0 0 0
""") == "5"

# mixed MSB groups
assert run("""1
6
1 2 4 8 3 7
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | 5 | nhóm chỉ có 0 | 
| quyền lực hỗn hợp | 2 | tách bằng MSB | 
| trộn mẫu | 3,1,1 | tính đúng đắn của logic nhóm | 

## Vỏ cạnh 

Một mảng không nặng kiểm tra khả năng xử lý đặc biệt của các số mà không có bất kỳ bit nào được đặt. Trong trường hợp như vậy, mọi phần tử đều thuộc nhóm 0 và thuật toán trả về chính xác tổng số vì không tồn tại cặp xung đột nào. 

Trường hợp cạnh thứ hai liên quan đến các số trải rộng trên nhiều cấp độ bit, chẳng hạn như lũy thừa của hai trộn với số nhị phân dày đặc. Mặc dù giá trị của chúng có thể gần bằng nhau về mặt số lượng nhưng MSB của chúng lại khác nhau, buộc chúng phải chia thành các nhóm riêng biệt. Thuật toán tách biệt chính xác từng nhóm và tránh các kết hợp không hợp lệ.
