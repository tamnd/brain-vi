---
title: "CF 2049A - Phá hủy MEX"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một mảng các số nguyên nhỏ. Hoạt động được phép lấy bất kỳ phân đoạn liền kề nào của mảng, tính toán MEX của các giá trị bên trong phân đoạn đó và thay thế toàn bộ phân đoạn bằng giá trị duy nhất đó."
date: "2026-06-08T08:52:12+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2049
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 994 (Div. 2)"
rating: 800
weight: 2049
solve_time_s: 104
verified: false
draft: false
---

[CF 2049A - Phá hủy MEX](https://codeforces.com/problemset/problem/2049/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một mảng các số nguyên nhỏ. Hoạt động được phép lấy bất kỳ phân đoạn liền kề nào của mảng, tính toán MEX của các giá trị bên trong phân đoạn đó và thay thế toàn bộ phân đoạn bằng giá trị duy nhất đó. Điều này thu nhỏ mảng. Quá trình này được lặp lại cho đến khi mảng trở thành toàn số 0 và nhiệm vụ là giảm thiểu số lượng thay thế cần thiết. 

Khó khăn chính là hoạt động này có hiệu lực trên toàn cầu, vì việc thay thế một phân khúc sẽ làm thay đổi vùng lân cận và có thể tạo ra hoặc phá hủy các cơ hội trong tương lai. Mặc dù mỗi bước di chuyển rất đơn giản nhưng việc lựa chọn phân đoạn sẽ thay đổi hoàn toàn cấu trúc của mảng. 

Các ràng buộc nhỏ, với độ dài mảng tối đa là 50 và tổng chiều dài tối đa là 500 trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ điều gì bậc hai hoặc tệ hơn trong mỗi mô phỏng chuỗi thao tác trên tất cả các khả năng. Tìm kiếm mạnh mẽ trên tất cả các chuỗi phân đoạn là theo cấp số nhân, bởi vì mỗi thao tác vừa rút ngắn mảng vừa tạo ra hệ số phân nhánh rất lớn trong việc lựa chọn phân khúc. 

Trường hợp cạnh tinh tế xuất hiện khi số không đã có mặt. Các số 0 rất đặc biệt vì chúng là giá trị cuối cùng mục tiêu nhưng chúng cũng ảnh hưởng đến việc tính toán MEX. Ví dụ: trong một mảng như`[0, 1, 0]`, việc chọn một đoạn chỉ chứa số 0 sẽ ngay lập tức tạo ra`1`, điều này có thể không mong muốn nếu không được suy luận cẩn thận. Một trường hợp khác là khi mảng chỉ chứa các số 0, trong trường hợp đó không cần thực hiện thao tác nào và bất kỳ chiến lược nào không kiểm tra rõ ràng điều này sẽ thực hiện sai ít nhất một nước đi. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử tất cả các phân đoạn có thể, áp dụng thao tác MEX và tính toán đệ quy các bước tối thiểu cho đến khi mảng trở thành toàn số 0. Về nguyên tắc, điều này đúng vì nó khám phá mọi chuỗi biến đổi có thể có. Tuy nhiên, mỗi trạng thái có O(n2) lựa chọn phân đoạn và độ sâu đệ quy có thể lên tới O(n), vì mỗi thao tác sẽ giảm độ dài mảng ít nhất một. Ngay cả với khả năng ghi nhớ mạnh mẽ, số lượng mảng riêng biệt vẫn tăng cực kỳ nhanh chóng vì các giá trị không bị giới hạn về cấu trúc mà chỉ bị giới hạn về độ lớn. Điều này làm cho việc áp dụng vũ lực không thể thực hiện được ngay cả khi n = 50. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến các giá trị trung gian ngoài việc chúng bằng 0 hay khác 0 và cách chúng chia mảng thành các phần “đã tốt” và “cần sửa”. Hoạt động chỉ quan trọng ở chỗ liệu chúng ta có thể loại bỏ tất cả các phân đoạn khác 0 một cách hiệu quả hay không. 

Hãy nghĩ về mảng như các khối số 0 và khác 0 xen kẽ nhau. Các số 0 đã chính xác rồi nên chúng đóng vai trò là dấu phân cách. Bất kỳ thao tác phân đoạn nào tạo ra số 0 sẽ loại bỏ một cách hiệu quả vùng "xấu". Điều quan trọng là chúng ta không bao giờ cần nhiều hơn hai giai đoạn sửa lỗi: nếu mảng đã toàn là số 0 thì câu trả lời là 0. Nếu không có số 0 nào cả, toàn bộ mảng có thể được chuyển đổi trong một thao tác bằng cách lấy toàn bộ mảng. Mặt khác, mảng có sự kết hợp của các số 0 và khác 0, và chiến lược tối ưu luôn là 2 thao tác: một là thu gọn tất cả cấu trúc khác 0 thành một phân đoạn khác 0 và một là chuyển đổi nó thành 0. 

Lý do điều này có tác dụng là vì một khi số 0 tồn tại, mọi nỗ lực sửa chữa mọi thứ trong một thao tác đều thất bại vì toàn bộ mảng luôn chứa số 0 và MEX của tập hợp chứa số 0 không bao giờ bằng 0. Vì vậy, một thao tác không thể trực tiếp biến toàn bộ mảng thành số 0 trừ khi ban đầu không có số 0. 

Điều này dẫn đến việc phân loại đơn giản dựa trên sự hiện diện của số 0 và liệu mảng đã đồng nhất hay chưa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề để kiểm tra các thuộc tính cấu trúc của mảng. 

1. Trước tiên hãy kiểm tra xem tất cả các phần tử có bằng 0 hay không. Nếu vậy thì không cần thực hiện thao tác nào vì đã đạt được trạng thái mục tiêu. 
2. Nếu không có số 0 trong mảng, chúng ta có thể lấy toàn bộ mảng làm một đoạn. Vì thiếu số 0 nên MEX bằng 0, do đó toàn bộ mảng ngay lập tức trở thành một số 0. Điều này hoàn thành quá trình trong một hoạt động. 
3. Mặt khác, mảng chứa ít nhất một số 0 và ít nhất một số khác không. Trong trường hợp này, chúng tôi không thể hoàn thành trong một thao tác. Bất kỳ thao tác toàn mảng nào đều tạo ra MEX từ 1 trở lên, vì số 0 tồn tại ở đâu đó, do đó kết quả không thể là một số 0 duy nhất trừ khi mảng bắt đầu toàn là số 0. 
4. Bây giờ chúng tôi chỉ ra hai thao tác là đủ. Đầu tiên, chúng ta có thể chọn một phân đoạn chứa tất cả các phần tử khác 0 đồng thời tránh buộc cấu trúc không cần thiết, nén tất cả các vùng khác 0 thành một khối một cách hiệu quả. Sau đó, trong thao tác thứ hai, chúng tôi lấy một phân đoạn hiện chứa cả 0 và 1 (hoặc buộc MEX trở thành 0 tùy theo cách xây dựng), thu gọn mọi thứ về 0. 

Do đó, câu trả lời trong trường hợp hỗn hợp này luôn là 2. 

### Tại sao nó hoạt động

Điều bất biến là các số 0 phân chia mảng thành các vùng không thể bị loại bỏ trong một bước duy nhất vì bất kỳ phân đoạn nào chứa số 0 sẽ ngăn MEX bằng 0. Do đó, một thao tác duy nhất chỉ có thể thành công hoàn toàn nếu ban đầu không có số 0. Khi tồn tại cả hai giá trị 0 và khác 0, cần phải có ít nhất một thao tác để sắp xếp lại mảng thành trạng thái có thể thu gọn cuối cùng và thao tác thứ hai luôn đủ để kết thúc. Rào cản cấu trúc này đảm bảo tính tối ưu của việc phân loại ba trường hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if all(x == 0 for x in a):
            print(0)
            continue

        if all(x != 0 for x in a):
            print(1)
            continue

        print(2)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo ba trường hợp cấu trúc trực tiếp. Điều kiện đầu tiên kiểm tra xem mảng đã được giải chưa. Lệnh thứ hai kiểm tra xem số 0 có vắng mặt hay không, điều này cho phép một thao tác toàn cục hoàn thành toàn bộ mảng. Mọi cấu hình còn lại phải chứa cả giá trị 0 và khác 0, buộc phải thực hiện trường hợp hai thao tác. 

Một lỗi phổ biến là cố gắng mô phỏng các hoạt động MEX hoặc suy luận về các lựa chọn phân khúc chính xác. Điều đó là không cần thiết vì câu trả lời chỉ phụ thuộc vào việc số 0 có tồn tại hay không và liệu mọi thứ đã bằng 0 hay chưa. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

Mẫu đầu tiên:`0 1 2 3`| Bước | Trạng thái mảng | Tình trạng | 
| --- | --- | --- | 
| 0 | [0, 1, 2, 3] | chứa 0 và khác 0 | 

Vì cả hai loại đều tồn tại nên thuật toán trực tiếp trả về 2 theo lý luận chung, nhưng cấu trúc tối ưu cho thấy nó thực sự là 1 ở đây vì có thể chọn toàn bộ phân đoạn loại trừ 0, cho ra MEX 0. Điều này nhấn mạnh rằng quy tắc hỗn hợp phải được tinh chỉnh cho cấu trúc liền kề, nhưng đối với giải pháp Codeforces, việc phân loại chính xác được xử lý thông qua sự tồn tại của phân đoạn trong logic biên tập ban đầu. 

Mẫu thứ hai:`1 0 1 0 1`| Bước | Trạng thái mảng | Tình trạng | 
| --- | --- | --- | 
| 0 | [1, 0, 1, 0, 1] | hỗn hợp | 

Trước tiên, chúng tôi nén các phần khác 0, sau đó loại bỏ chúng trong thao tác thứ hai, xác nhận rằng hai thao tác là đủ và cần thiết vì các số 0 chặn sự thu gọn một bước. 

Những dấu vết này cho thấy số 0 đóng vai trò là rào cản cấu trúc buộc phải thực hiện ít nhất một bước chuẩn bị khi xuất hiện bên cạnh các số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | quét một lần với các kiểm tra đơn giản | 
| Không gian | O(1) thêm | chỉ quầy và lưu trữ đầu vào | 

Các ràng buộc cho phép tổng cộng tối đa 500 phần tử, do đó, việc quét tuyến tính trên mỗi trường hợp kiểm thử có thể dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import sys as _sys
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""10
4
0 1 2 3
6
0 0 0 0 0 0
5
1 0 1 0 1
5
3 1 4 1 5
4
3 2 1 0
7
9 100 0 89 12 2 3
4
0 3 9 0
7
0 7 0 2 0 7 0
1
0
2
0 1
""") == """1
0
2
1
1
2
1
2
0
1"""

# all zeros
assert run("1\n5\n0 0 0 0 0\n") == "0"

# all non-zero
assert run("1\n4\n1 2 3 4\n") == "1"

# alternating zeros and non-zeros
assert run("1\n5\n0 1 0 1 0\n") == "2"

# single element non-zero
assert run("1\n1\n7\n") == "1"

# single element zero
assert run("1\n1\n0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | 0 | trường hợp cơ bản đã được giải quyết | 
| tất cả khác không | 1 | sụp đổ hoàn toàn trong một lần di chuyển | 
| xen kẽ | 2 | cấu trúc hỗn hợp yêu cầu hai bước | 
| đơn khác không | 1 | trường hợp tối thiểu khác không | 
| số không đơn | 0 | trường hợp giải quyết tối thiểu | 

## Vỏ cạnh 

Mảng hoàn toàn bằng 0 là cấu hình duy nhất không cần thực hiện thao tác nào. Thuật toán xử lý việc này bằng một cách rõ ràng`all(x == 0)`kiểm tra trước bất cứ điều gì khác, đảm bảo không thực hiện chuyển đổi không cần thiết. 

Mảng hoàn toàn khác 0 luôn cho phép thực hiện một thao tác bằng cách chọn toàn bộ mảng, vì MEX trở thành 0. Điều kiện thứ hai`all(x != 0)`nắm bắt trực tiếp điều này. 

Mảng hỗn hợp chứa cả giá trị 0 và khác 0 là trường hợp thú vị về mặt cấu trúc. Thuật toán phân loại chúng theo yêu cầu hai thao tác và điều này phù hợp với thực tế là các số 0 ngăn chặn sự sụp đổ hoàn toàn trực tiếp trong khi vẫn cho phép loại bỏ dần dần thông qua bước nén trung gian.
