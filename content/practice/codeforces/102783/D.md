---
title: "CF 102783D - Ghost-or-Treat"
description: "Chúng ta có một dòng ma, mỗi con có một số nguyên tuổi. Một động thái bao gồm việc chọn hai hồn ma lân cận có độ tuổi khác nhau. Con ma trẻ biến mất, trong khi con ma lớn tuổi vẫn ở lại và già đi một tuổi."
date: "2026-07-27T19:58:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102783
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 2"
rating: 0
weight: 102783
solve_time_s: 51
verified: true
draft: false
---

[CF 102783D - Ghost-or-Treat](https://codeforces.com/problemset/problem/102783/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dòng ma, mỗi con có một số nguyên tuổi. Một động thái bao gồm việc chọn hai hồn ma lân cận có độ tuổi khác nhau. Con ma trẻ biến mất, trong khi con ma lớn tuổi vẫn ở lại và già đi một tuổi. Thứ tự của các hồn ma còn lại vẫn giữ nguyên sau khi hồn ma rời đi. 

Nhiệm vụ không phải là mô phỏng quá trình. Chúng ta chỉ cần quyết định xem liệu Boo có thể chọn nước đi theo thứ tự nào đó sao cho còn lại đúng một con ma hay không. 

Dữ liệu đầu vào cung cấp số lượng ma theo sau là độ tuổi của chúng theo thứ tự dòng. Đầu ra là CÓ nếu tồn tại một chuỗi các kết quả khớp hợp lệ loại bỏ mọi bóng ma ngoại trừ một bóng ma và KHÔNG nếu ngược lại. 

Hạn chế quan trọng là số lượng bóng ma có thể lên tới 10000. Việc mô phỏng tất cả các lựa chọn có thể xảy ra là không thể vì số lượng đơn đặt hàng phù hợp có thể tăng lên cực kỳ nhanh chóng. Ngay cả việc thử từng cặp lựa chọn lân cận cũng dẫn đến một không gian tìm kiếm theo cấp số nhân. Chúng ta cần một thuộc tính lâu đời có thể quyết định trực tiếp câu trả lời. 

Các trường hợp cạnh nhỏ nhưng dễ bỏ sót. Nếu chỉ có một con ma thì câu trả lời ngay là CÓ vì đã có đúng một con ma còn lại. 

Ví dụ:```
Input
1
7

Output
YES
```Một giải pháp chỉ kiểm tra xem mọi lứa tuổi có bằng nhau hay không có thể từ chối trường hợp này một cách không chính xác. 

Một trường hợp quan trọng khác là khi mọi hồn ma đều có tuổi bằng nhau và có nhiều hơn một hồn ma.```
Input
4
5
5
5
5

Output
NO
```Không có hai con ma liền kề nào có tuổi khác nhau nên nước đi đầu tiên thậm chí không thể bắt đầu. 

Trường hợp cuối cùng là khi các độ tuổi bằng nhau không có cùng giá trị.```
Input
5
4
4
3
4
4

Output
YES
```Một cách tiếp cận bất cẩn có thể cho rằng các giá trị lặp đi lặp lại ngăn cản sự tiến bộ, nhưng cậu bé trung niên 3 tuổi có thể chiến đấu với một người hàng xóm, cho phép một con ma phát triển và cuối cùng loại bỏ những con ma khác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi chuỗi trận chiến có thể xảy ra. Đối với mỗi trạng thái, chúng tôi chọn mọi cặp liền kề có độ tuổi khác nhau, loại bỏ cặp nhỏ hơn, tăng cặp lớn hơn và kiểm tra đệ quy xem liệu chúng tôi có thể tiếp cận được một bóng ma hay không. Đây chính xác là mô hình hóa quy trình, vì vậy nó đúng, nhưng số lượng trạng thái quá lớn. Với 10000 con ma, ngay cả những lựa chọn đầu tiên cũng đã tạo ra một cây phân nhánh khổng lồ. 

Nhận xét quan trọng là tình huống duy nhất mà cuộc chiến không thể bắt đầu là khi mọi hồn ma đều có cùng độ tuổi. Nếu có ít nhất hai độ tuổi khác nhau, chúng ta luôn có thể tiến bộ. 

Giả sử có một con ma có tuổi khác với một số con ma khác. Hãy cân nhắc việc di chuyển qua hàng bằng cách liên tục chiến đấu với một con ma hiện khác với hàng xóm. Con ma già hơn trong mỗi cuộc chiến vẫn sống sót và tăng tuổi. Khi một con ma đã trở nên già hơn một con ma khác, nó có thể tiếp tục đánh bại những con ma hàng xóm trẻ hơn. Các độ tuổi bằng nhau có thể bị bỏ qua cho đến khi một hồn ma mạnh hơn tiếp cận họ, bởi vì khi đó hồn ma mạnh hơn sẽ có một cuộc chiến hợp lệ. Cuối cùng, một con ma có thể hấp thụ tất cả những con ma khác. 

Toàn bộ vấn đề quy về việc kiểm tra xem mọi lứa tuổi có giống nhau hay không, với trường hợp đặc biệt là một con ma đã đáp ứng được mục tiêu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mọi lứa tuổi ma. Nếu chỉ có một bản ghost, xuất CÓ vì không cần kết quả trùng khớp. 
2. So sánh mọi thời đại với tuổi ma đầu tiên. Nếu tất cả các độ tuổi bằng nhau, xuất ra NO vì không có cặp liền kề nào có thể có độ tuổi khác nhau. 
3. Nếu có ít nhất một độ tuổi khác với độ tuổi đầu tiên, ghi CÓ vì sự tồn tại của hai độ tuổi khác nhau đảm bảo rằng quy trình có thể được sắp xếp để chỉ còn lại một người sống sót. 

Lý do điều này có tác dụng là vì thứ tự chính xác của các trận đấu không quan trọng đối với quyết định. Một mảng không đồng nhất luôn chứa điểm khởi đầu cần thiết để tạo ra một con ma trở nên già hơn sau mỗi trận chiến thành công. Sự trưởng thành chỉ giúp ích cho những cuộc chiến trong tương lai vì một hồn ma sống sót không bao giờ trẻ hơn. 

Tại sao nó hoạt động: 

Nếu tất cả các con ma có cùng độ tuổi và có nhiều hơn một con ma thì mọi phép so sánh liền kề đều thất bại, do đó không có nước đi nào tồn tại. 

Nếu không thì có hai hồn ma ở độ tuổi khác nhau. Người lớn tuổi hơn có thể đánh bại người trẻ hơn nếu họ ở gần nhau và sau khi chiến thắng, người đó sẽ tăng tuổi của mình. Tuổi tác ngày càng tăng không bao giờ có thể khiến cuộc chiến trong tương lai trở nên khó khăn hơn vì tuổi lớn hơn chỉ tạo ra nhiều đối thủ khả dĩ hơn. Việc lặp lại ý tưởng này cho phép một hồn ma sống sót trong tất cả các trận đấu. Vì vậy, tình huống bất khả thi duy nhất là có nhiều hồn ma có độ tuổi giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ages = [int(input()) for _ in range(n)]

    if n == 1:
        print("YES")
        return

    first = ages[0]
    for age in ages[1:]:
        if age != first:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp chỉ lưu trữ độ tuổi vì dữ liệu đầu vào được cung cấp một giá trị trên mỗi dòng và cách thực hiện đơn giản nhất là đọc chúng thành một mảng. Thuật toán thực tế chỉ cần tuổi đầu tiên và liệu có tồn tại tuổi khác hay không, do đó, ý tưởng tương tự có thể được thực hiện với bộ nhớ bổ sung liên tục. 

các`n == 1`kiểm tra xuất hiện trước kiểm tra đẳng thức vì một con ma ở mọi lứa tuổi đã là trạng thái cuối cùng hợp lệ. Đối với đầu vào lớn hơn, việc tìm ra một độ tuổi khác nhau là đủ để chứng minh rằng có tồn tại một chuỗi các cuộc chiến. 

Không có phép toán số học nào liên quan đến các giá trị lớn, do đó việc tràn số nguyên không phải là vấn đề đáng lo ngại trong Python. Việc triển khai cũng tránh mô phỏng các cuộc chiến, vốn là nguyên nhân chính gây ra sự phức tạp không cần thiết. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
Input
5
5
3
4
4
5
```Những thay đổi biến quan trọng là: 

| Bước | Đã kiểm tra tuổi hiện tại | Tuổi đầu tiên | Quyết định | 
| --- | --- | --- | --- | 
| Bắt đầu | 5 | 5 | Tiếp tục | 
| Đọc 3 | 3 | 5 | Tìm thấy giá trị khác nhau | 
| Kết thúc | | | Đầu ra CÓ | 

Dấu vết này thể hiện quan sát chính. Thuật toán dừng ngay khi tìm thấy điểm khởi đầu có thể có. 

Đối với mẫu thứ hai:```
Input
3
1
1
1
```Dấu vết là: 

| Bước | Đã kiểm tra tuổi hiện tại | Tuổi đầu tiên | Quyết định | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 1 | Tiếp tục | 
| Đọc 1 | 1 | 1 | Tiếp tục | 
| Đọc 1 | 1 | 1 | Tất cả các giá trị bằng nhau | 
| Kết thúc | | | Đầu ra KHÔNG | 

Điều này xác nhận trường hợp bị chặn khi không có nước đi đầu tiên hợp lệ nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi tuổi ma đều được kiểm tra nhiều nhất một lần. | 
| Không gian | O(N) | Việc thực hiện lưu trữ độ tuổi đầu vào. | 

Các ràng buộc cho phép quét tuyến tính một cách thoải mái. Thuật toán chỉ thực hiện các so sánh đơn giản nên dễ dàng phù hợp trong giới hạn thời gian. Mảng được lưu trữ cũng có thể bị xóa để giảm mức sử dụng bộ nhớ xuống O(1), nhưng phiên bản hiện tại đã nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# sample 1
assert run("""5
5
3
4
4
5
""") == "YES\n", "sample 1"

# sample 2
assert run("""3
1
1
1
""") == "NO\n", "sample 2"

# single ghost
assert run("""1
100
""") == "YES\n", "single ghost"

# all equal values
assert run("""6
8
8
8
8
8
8
""") == "NO\n", "all equal"

# different values separated by equal values
assert run("""5
4
4
3
4
4
""") == "YES\n", "middle different value"

# large value boundary
assert run("""2
1000000000
999999999
""") == "YES\n", "large ages"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một con ma | CÓ | Xử lý trường hợp đã hoàn thành. | 
| Mọi lứa tuổi đều bình đẳng | KHÔNG | Khẳng định rằng không thể thực hiện được bước đi đầu tiên. | 
| Một giá trị khác nhau giữa các giá trị bằng nhau | CÓ | Xác nhận rằng độ tuổi lặp đi lặp lại không chặn quá trình. | 
| Độ tuổi rất lớn | CÓ | Xác nhận rằng kích thước tuổi không ảnh hưởng đến logic. | 

## Vỏ cạnh 

Đối với trường hợp ma đơn:```
Input
1
7
```Thuật toán kiểm tra`n == 1`ngay lập tức và trả về CÓ. Không cần phải đánh nhau vì mục tiêu đã được thỏa mãn. 

Đối với trường hợp hoàn toàn bằng nhau:```
Input
4
5
5
5
5
```Quá trình quét so sánh mọi giá trị với độ tuổi đầu tiên. Vì mọi so sánh đều bằng nhau nên thuật toán trả về KHÔNG. Điều này khớp với quy trình vì không có cặp liền kề nào có độ tuổi khác nhau, do đó Boo không thể thực hiện dù chỉ một thao tác. 

Đối với trường hợp có các nhóm bằng nhau ở độ tuổi khác nhau:```
Input
5
4
4
3
4
4
```Quá trình quét cho thấy con ma thứ ba có 3 tuổi trong khi con ma thứ nhất có 4 tuổi. Điều này chứng tỏ đường dây không bị đóng băng. Con ma lớn tuổi cuối cùng có thể sử dụng con ma trẻ hơn để tăng tuổi và tiếp tục giành chiến thắng trong các trận chiến, vì vậy câu trả lời là CÓ. 

Đối với trường hợp các độ tuổi khác nhau ở cuối:```
Input
3
10
10
20
```Thuật toán tìm giá trị cuối cùng khác với giá trị đầu tiên và trả về CÓ. Con ma cuối cùng không cần phải bắt đầu bên cạnh mọi con ma khác. Sự tồn tại của bất kỳ sự khác biệt nào là đủ bởi vì các cuộc chiến có thể được sắp xếp để đưa một người sống sót ngày càng tăng vượt qua ranh giới.
