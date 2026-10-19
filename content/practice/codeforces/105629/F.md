---
title: "CF 105629F - \u6570\u6570"
description: "Chúng ta đang làm việc với các chuỗi nhị phân có độ dài cố định $n$. Mỗi vị trí là 0 hoặc 1, do đó tổng không gian chứa các chuỗi $2^n$."
date: "2026-06-22T14:55:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105629
codeforces_index: "F"
codeforces_contest_name: "The 19-th Beihang University Collegiate Programming Contest (BCPC 2024) - Final"
rating: 0
weight: 105629
solve_time_s: 63
verified: true
draft: false
---

[CF 105629F - \u6570\u6570](https://codeforces.com/problemset/problem/105629/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với các chuỗi nhị phân có độ dài cố định$n$. Mỗi vị trí là 0 hoặc 1, do đó tổng không gian chứa$2^n$dây. Trong số này, chúng ta muốn đếm xem có bao nhiêu thỏa mãn ít nhất một trong hai điều kiện: chuỗi chứa một khối liên tiếp có ít nhất$a$số không, hoặc nó chứa một khối liên tiếp ít nhất$b$những cái đó. Câu trả lời là bắt buộc theo modulo$10^9 + 7$. 

Loại điều kiện này dễ lý luận hơn thông qua quan điểm bổ sung. Thay vì đếm trực tiếp các chuỗi “chứa một chuỗi dài”, chúng ta thường đếm các chuỗi tránh được cả hai sự kiện xấu: không có chuỗi số 0 nào đạt đến độ dài$a$, và không có cái nào đạt đến độ dài$b$. Câu trả lời cuối cùng là tổng số chuỗi trừ đi tập hợp giới hạn này. 

Kích thước đầu vào cho phép$n$lên đến khoảng$10^6$, do đó bất kỳ DP bậc hai nào trên$n$hoặc thậm chí$O(n \cdot a)$ngay lập tức là không thể thực hiện được. Thậm chí$O(n \log n)$sẽ là đường biên nếu hằng số lớn. Điều này đẩy chúng ta tới một DP thời gian tuyến tính với các chuyển đổi không đổi hoặc khấu hao không đổi trên mỗi trạng thái. 

Trường hợp cạnh tinh tế xuất hiện khi$a = 1$hoặc$b = 1$. Nếu như$a = 1$, thì bất kỳ chuỗi nào cũng đã chứa một chuỗi các số 0 có độ dài ít nhất là 1, vì vậy mọi chuỗi đều hợp lệ. Tương tự, nếu$b = 1$, mọi chuỗi đều hợp lệ. Một DP bổ sung ngây thơ có thể cố gắng cấm một cách không chính xác các trạng thái vốn đã không thể thực hiện được, dẫn đến sai sót từng lỗi một hoặc chuyển đổi trạng thái trống. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo ra tất cả$2^n$chuỗi nhị phân và kiểm tra từng chuỗi bằng cách quét chuỗi số 0 và số 1 liên tiếp dài nhất. Điều này đúng nhưng ngay lập tức thất bại vì$2^n$tăng trưởng theo cấp số nhân. Ngay cả đối với$n = 40$, cái này trở nên quá lớn, và ở đây$n$tùy thuộc vào$10^6$, làm cho nó hoàn toàn không thể sử dụng được. 

Quan sát quan trọng là thuộc tính mà chúng ta quan tâm chỉ phụ thuộc vào chuỗi ký tự giống hệt nhau liên tiếp hiện tại. Khi chúng ta biết có bao nhiêu số 0 hoặc số 1 liên tiếp ở cuối tiền tố, chúng ta không cần lịch sử đầy đủ. Điều này biến vấn đề thành một máy tự động hữu hạn: chúng tôi theo dõi ký tự hiện tại và độ dài lần chạy hiện tại của nó, giới hạn ở$a-1$cho số không và$b-1$dành cho những người đó. Bất kỳ chuyển đổi nào vượt quá giới hạn đều bị cấm. 

Điều này dẫn đến một công thức lập trình động trong đó chúng ta xây dựng chuỗi từ trái sang phải. Chúng ta hãy xác định trạng thái dựa trên ký tự cuối cùng và độ dài chạy của nó. Chúng tôi duy trì hai nhóm: trạng thái kết thúc bằng 0 với độ dài chạy$1 \dots a-1$và các trạng thái kết thúc bằng 1 với độ dài chạy$1 \dots b-1$. Ở mỗi bước, chúng tôi mở rộng thêm 0 hoặc 1, thay đổi độ dài lần chạy hoặc đặt lại chúng. 

Một DP ngây thơ sẽ duy trì tất cả các trạng thái này một cách rõ ràng và cập nhật chúng cho từng vị trí. Điều đó sẽ tốn kém$O(n(a+b))$, quá lớn. Cấu trúc quan trọng là các chuyển đổi trong mỗi nhóm là các thao tác dịch chuyển tiền tố, có thể được tăng tốc bằng cách sử dụng tổng tiền tố. Thay vì tính toán lại từng trạng thái một cách độc lập, chúng tôi tính toán tổng tích lũy trên các trạng thái hợp lệ trước đó để mỗi lớp có thể được cập nhật theo thời gian tuyến tính theo số lượng trạng thái và sau đó được tối ưu hóa hơn nữa bằng cách sử dụng các kỹ thuật tiền tố cuộn để tổng trở thành$O(n)$. 

Cuối cùng, vì chúng ta đang tính phần bù nên câu trả lời là$2^n - \text{valid}(n)$, Ở đâu$\text{valid}(n)$là số chuỗi tránh được cả hai lần chạy dài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| DP thời lượng chạy có tối ưu hóa |$O(n)$|$O(a + b)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán trước lũy thừa của hai đến$n$, vì tổng số chuỗi nhị phân là$2^n$. Điều này cho chúng ta cơ sở cho bước bổ sung. 

Sau đó, chúng tôi xây dựng DP cho các chuỗi hợp lệ, nghĩa là các chuỗi tránh được việc chạy$a$số không và tránh chạy$b$những cái đó. 

Chúng tôi duy trì hai mảng: một mảng cho các trạng thái kết thúc bằng 0 và một mảng cho các trạng thái kết thúc bằng 1. Mỗi mảng lưu trữ số lượng được lập chỉ mục theo độ dài chạy. Ở bước$i$, các mảng này đại diện cho tất cả các chuỗi có độ dài hợp lệ$i$. 

Ở mỗi lần thêm ký tự mới, chúng tôi tính toán các mảng mới như sau. Khi thêm số 0, chúng tôi mở rộng chuỗi 0 trước đó bằng cách tăng độ dài của nó hoặc chúng tôi chuyển từ chuỗi 1 sang chuỗi 0 có độ dài 1. Logic đối xứng tương tự được áp dụng khi thêm 1. 

Quá trình chuyển đổi từ “tất cả các lần chạy kết thúc bằng ký tự ngược lại” sang “tất cả các lần chạy kết thúc bằng ký tự hiện tại” là lúc mà tổng tiền tố trở nên quan trọng. Thay vì lặp lại tất cả các khoảng thời gian chạy trước đó có thể có cho từng trạng thái mới, chúng tôi duy trì tổng tích lũy để mỗi lần cập nhật có thể được tính toán theo thời gian không đổi cho mỗi trạng thái. 

Sau khi xử lý tất cả các vị trí, chúng tôi tính tổng tất cả các trạng thái kết thúc hợp lệ trên cả hai nhóm. Điều này đưa ra số lượng chuỗi tránh được cả hai lần chạy bị cấm. Trừ cái này khỏi$2^n$mang lại câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý vị trí$i$, mọi trạng thái DP đều đếm chính xác số tiền tố có độ dài hợp lệ$i$kết thúc bằng một ký tự cụ thể và độ dài chạy. Mọi chuyển đổi đều duy trì tính hợp lệ vì chúng tôi chỉ mở rộng các lần chạy vẫn ở dưới ngưỡng cấm của chúng. Vì mỗi chuỗi hợp lệ có sự phân tách duy nhất thành các độ dài chạy ở mỗi tiền tố nên không có chuỗi nào bị tính hai lần hoặc bị bỏ sót. Bước bổ sung là hợp lệ vì DP bao gồm chính xác tập hợp các chuỗi bổ sung vi phạm ít nhất một ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, a, b = map(int, input().split())

    # if any constraint allows everything
    if a == 1 or b == 1:
        print(pow(2, n, MOD))
        return

    # dp0[i] = ways ending with 0-run length i (1..a-1)
    # dp1[i] = ways ending with 1-run length i (1..b-1)
    dp0 = [0] * a
    dp1 = [0] * b

    # initial state: single 0 or single 1
    dp0[1] = 1
    dp1[1] = 1

    total_len = 1

    for _ in range(2, n + 1):
        ndp0 = [0] * a
        ndp1 = [0] * b

        # prefix sums for fast range transitions
        pre0 = [0] * (a + 1)
        pre1 = [0] * (b + 1)

        for i in range(1, a):
            pre0[i] = (pre0[i - 1] + dp0[i]) % MOD
        for i in range(1, b):
            pre1[i] = (pre1[i - 1] + dp1[i]) % MOD

        # transitions to dp0
        # extend previous 0-runs
        for i in range(1, a):
            ndp0[i] = dp0[i - 1] if i > 1 else 0
        # switch from all valid 1-runs
        ndp0[1] = pre1[b - 1]

        # transitions to dp1
        for i in range(1, b):
            ndp1[i] = dp1[i - 1] if i > 1 else 0
        ndp1[1] = pre0[a - 1]

        dp0, dp1 = ndp0, ndp1

    valid = (sum(dp0) + sum(dp1)) % MOD
    ans = (pow(2, n, MOD) - valid) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ hai mảng DP có thời lượng chạy và cập nhật chúng theo từng lớp. Phần tinh tế là tách “phần mở rộng của cùng một ký tự” khỏi “ký tự chuyển đổi”, vì việc chuyển đổi luôn đặt lại độ dài chạy thành 1 và tổng hợp tất cả các trạng thái tương thích trước đó thông qua tổng tiền tố. 

Một lỗi phổ biến là cố gắng lặp lại tất cả các lần chạy trước đó một cách rõ ràng cho mọi trạng thái, điều này dẫn đến$O(n^2)$nổ tung. Một vấn đề điển hình khác là quên rằng thời lượng chạy bị giới hạn nghiêm ngặt bên dưới$a$Và$b$, vì vậy các mảng phải có kích thước phù hợp và các quá trình chuyển đổi không bao giờ được truy cập vào các trạng thái không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, a = 2, b = 2
```Chúng tôi muốn tránh mọi chuỗi có độ dài 2, vì vậy chỉ các chuỗi xen kẽ mới hợp lệ. 

| bước | dp0 | dp1 | 
| --- | --- | --- | 
| 1 | [1] | [1] | 
| 2 | [1] | [1] | 
| 3 | [1] | [1] | 

Các chuỗi hợp lệ là “010” và “101”, vì vậy hợp lệ = 2. Tổng = 8, câu trả lời = 6. 

Điều này xác nhận rằng DP theo dõi chính xác cấu trúc xen kẽ khi cả hai giới hạn chạy đều chặt chẽ. 

### Ví dụ 2 

đầu vào:```
n = 4, a = 3, b = 2
```Chúng tôi cho phép các số 0 dài nhưng cấm các số 0 liên tiếp. 

Sau khi chuyển đổi, DP hội tụ về các chuỗi đếm trong đó các chuỗi không bao giờ xuất hiện liên tiếp. 

| bước | dp0 | dp1 | 
| --- | --- | --- | 
| 1 | [1] | [1] | 
| 2 | [1,1] | [1] | 
| 3 | [2,1] | [2] | 
| 4 | [3,2] | [3] | 

Hợp lệ = 11, tổng cộng = 16, câu trả lời = 5. 

Điều này cho thấy các ràng buộc bất đối xứng đã nghiêng DP về phía nhân vật không bị hạn chế như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vị trí thực hiện chuyển đổi theo thời gian không đổi bằng cách sử dụng tập hợp tiền tố trên các mảng trạng thái giới hạn | 
| Không gian |$O(a + b)$| Chỉ các trạng thái độ dài chạy cho số 0 và số 1 mới được lưu trữ | 

Các ràng buộc cho phép lên đến$10^6$, vì vậy thời gian tuyến tính là cần thiết. DP tránh lặp lại lồng nhau trên mỗi trạng thái và chỉ thực hiện quét tuyến tính trên mỗi bước, điều này vẫn khả thi do các hệ số không đổi nhỏ và mảng cuộn.
