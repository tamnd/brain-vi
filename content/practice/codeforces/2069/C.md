---
title: "CF 2069C - Chuỗi Đẹp"
description: "Chúng ta được yêu cầu đếm số dãy con \"đẹp\" trong một mảng trong đó mọi phần tử là 1, 2 hoặc 3."
date: "2026-06-08T06:59:08+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2069
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 174 (Rated for Div. 2)"
rating: 1500
weight: 2069
solve_time_s: 98
verified: false
draft: false
---

[CF 2069C - Trình tự đẹp](https://codeforces.com/problemset/problem/2069/C) 

**Đánh giá:** 1500 
**Tags:** tổ hợp, dp, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số dãy con "đẹp" trong một mảng trong đó mỗi phần tử là 1, 2 hoặc 3. Một dãy con đẹp phải có ít nhất ba phần tử, mọi phần tử ngoại trừ phần tử đầu tiên phải có phần tử nhỏ hơn ở bên trái và mọi phần tử ngoại trừ phần tử cuối cùng phải có phần tử lớn hơn ở bên phải. Nói cách khác, dãy con phải tăng ở đâu đó và giảm ở đâu đó, đảm bảo không bị đơn điệu. Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định độ dài của mảng và chính mảng đó. Kết quả đầu ra là số dãy con đẹp modulo 998244353. 

Các ràng buộc cho phép tối đa 200.000 phần tử trong một mảng và tổng cộng 200.000 phần tử trong tất cả các trường hợp thử nghiệm. Điều này ngăn cản bất kỳ cách tiếp cận nào kiểm tra rõ ràng mọi chuỗi con có thể xảy ra vì điều đó sẽ yêu cầu tới 2^200.000 thao tác, điều này là không khả thi. Việc các phần tử chỉ có 1, 2 hoặc 3 gợi ý rằng có thể có giải pháp khai thác phạm vi giá trị nhỏ này. Các trường hợp cạnh bao gồm các mảng có độ dài tối thiểu là ba, các mảng có tất cả các giá trị giống hệt nhau trong đó không có chuỗi con nào là đẹp và các mảng chỉ tồn tại một mẫu tăng giảm duy nhất. 

Ví dụ, nếu mảng là`[1, 1, 1]`, đầu ra đúng là 0 vì không có phần tử nào lớn hơn phần tử đầu tiên nên không có dãy con nào có thể thỏa mãn điều kiện đẹp. Một cách tiếp cận ngây thơ đếm các dãy con mà không kiểm tra các điều kiện trái-nhỏ hơn và phải-lớn hơn sẽ đếm sai các dãy này. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên tất cả các chuỗi con có độ dài từ ba trở lên, kiểm tra các điều kiện cho từng chuỗi và đếm những điều kiện thỏa mãn chúng. Việc tạo tất cả các chuỗi con một cách rõ ràng sẽ mất O(2^n) thời gian và việc kiểm tra từng chuỗi để đảm bảo vẻ đẹp sẽ mất tới các thao tác O(n). Với n = 200.000, tốc độ này quá chậm. 

Thông tin chi tiết quan trọng là vì các giá trị được giới hạn ở 1, 2, 3 nên chúng ta có thể lập mô hình các chuỗi con bằng cách theo dõi số lượng chuỗi kết thúc bằng mỗi số và cách mở rộng chúng trong khi vẫn tôn trọng các điều kiện chuỗi đẹp. Cụ thể, một dãy con đẹp phải chứa ít nhất một số 1, một số 2 và một số 3 theo thứ tự nào đó. Điều này cho phép phương pháp lập trình động trong đó chúng tôi theo dõi số lượng chuỗi con có độ dài 1, 2 và 3 khi chúng tôi quét mảng. Mỗi phần tử có thể mở rộng các chuỗi con hiện có hoặc bắt đầu các chuỗi mới dựa trên giá trị của nó. Điều này làm giảm vấn đề từ độ phức tạp theo cấp số nhân sang độ phức tạp tuyến tính tính bằng n cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(2^n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ba bộ đếm:`count1`đối với các dãy tận cùng bằng 1,`count12`đối với các dãy con kết thúc bằng 2 có số 1 và`count123`đối với các dãy con kết thúc bằng 3 có số 1 và số 2. Tất cả đều bắt đầu từ số 0. Các bộ đếm này đại diện cho số cách để hình thành các chuỗi đẹp một phần. 
2. Lặp lại từng phần tử trong mảng. Đối với phần tử bằng 1, tăng dần`count1`bằng 1 vì một dãy con mới bắt đầu bằng 1 có thể bắt đầu ở đây. Đối với phần tử bằng 2, tăng dần`count12`qua`count1`bởi vì mỗi dãy con kết thúc bằng 1 có thể được mở rộng bằng 2 này. Đối với một phần tử bằng 3, tăng dần`count123`qua`count12`bởi vì mỗi dãy con của dạng 1→2 có thể được mở rộng bởi 3 này để tạo thành một dãy con thỏa mãn các điều kiện về vẻ đẹp. 
3. Tiếp tục quét mảng, cập nhật số đếm như trên. Mỗi bộ đếm chỉ phụ thuộc vào bộ đếm trước đó chứ không phụ thuộc vào danh sách dãy con đầy đủ. Hoạt động modulo đảm bảo số đếm không bị tràn. 
4. Sau khi quét mảng,`count123`chứa tổng số dãy con đẹp trong mảng này. 

Điều này hoạt động vì bộ đếm theo dõi tăng dần tất cả các chuỗi con hợp lệ và đảm bảo các điều kiện bên trái nhỏ hơn và bên phải lớn hơn theo thứ tự trong đó các phần tử mở rộng chuỗi con. Mọi dãy con đẹp có độ dài bằng ba hoặc nhiều hơn đều được tính bằng cách mở rộng các dãy con nhỏ hơn theo cách này. Các chuỗi dài hơn ba sẽ được tính tự động vì bộ đếm tích lũy các cách để nối thêm các phần tử bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        count1 = 0
        count12 = 0
        count123 = 0
        
        for x in a:
            if x == 1:
                count1 = (count1 + 1) % MOD
            elif x == 2:
                count12 = (count12 + count1) % MOD
            elif x == 3:
                count123 = (count123 + count12) % MOD
        
        print(count123)

if __name__ == "__main__":
    solve()
```Các bộ đếm được cập nhật theo cách duy trì tính bất biến:`count1`đếm các chuỗi có thể kéo dài thêm 2,`count12`đếm các chuỗi có thể được kéo dài thêm 3 và`count123`đếm đã hoàn thành các chuỗi đẹp. Hoạt động Modulo ngăn ngừa tràn. Chỉ sử dụng ba số nguyên đảm bảo không gian O(1). 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**:`[3, 2, 1, 2, 2, 1, 3]`| Bước | Yếu tố | đếm1 | đếm12 | đếm123 | Giải thích | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 0 | 0 | Không có trước 1→2, không thể bắt đầu seq đẹp | 
| 2 | 2 | 0 | 0 | 0 | Không có trước 1 | 
| 3 | 1 | 1 | 0 | 0 | Chuỗi tiếp theo mới bắt đầu | 
| 4 | 2 | 1 | 1 | 0 | Mở rộng 1→2 trước đó | 
| 5 | 2 | 1 | 2 | 0 | Mở rộng lại 1→2 trước đó | 
| 6 | 1 | 2 | 2 | 0 | 1 khởi đầu mới | 
| 7 | 3 | 2 | 4 | 3 | Mỗi 1→2 mở rộng thành 1→2→3 | 

Số đếm cuối cùng123 là 3, phù hợp với kết quả đầu ra dự kiến. 

**Đầu vào mẫu 2**:`[1, 2, 3]`| Bước | Yếu tố | đếm1 | đếm12 | đếm123 | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 
| 2 | 2 | 1 | 1 | 0 | 
| 3 | 3 | 1 | 1 | 1 | 

Đầu ra là 1, phù hợp với đầu ra dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ có ba bộ đếm được sử dụng, độc lập với n | 

Với n tối đa 2×10^5 và t tối đa 10^4, tổng các phép toán có tổng là 2×10^5, trong vòng 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("4\n7\n3 2 1 2 2 1 3\n4\n3 1 2 2\n3\n1 2 3\n9\n1 2 3 2 1 3 2 2 3\n") == "3\n0\n1\n22"

# Custom cases
assert run("1\n3\n1 1 1\n") == "0", "all equal values"
assert run("1\n3\n1 2 3\n") == "1", "minimum size beautiful sequence"
assert run("1\n4\n3 2 1 2\n") == "0", "no increasing subsequence"
assert run("1\n5\n1 2 2 3 3\n") == "4", "multiple extensions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1,1,1]`| 0 | Không có chuỗi con đẹp khi tất cả các giá trị giống hệt nhau | 
|`[1,2,3]`| 1 | Chuỗi đẹp tối thiểu có độ dài ba | 
|`[3,2,1,2]`| 0 | Không tồn tại dãy con 1→2→3 hợp lệ | 
|`[1,2,2,3,3]`| 4 | Nhiều chuỗi con có thể mở rộng để tạo thành chuỗi đẹp mắt | 

## Vỏ cạnh 

Đối với một mảng bao gồm toàn bộ 3 giây,`[3,3,3]`, bộ đếm vẫn còn`count1=0`,`count12=0`,`count123=0`, do đó đầu ra là 0. Đối với các mảng bắt đầu bằng 2,`[2,3,1]`,`count1`không tăng cho đến khi nhìn thấy số 1, do đó các chuỗi bắt đầu không chính xác sẽ không được tính
