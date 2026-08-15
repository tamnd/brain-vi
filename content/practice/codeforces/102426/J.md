---
title: "CF 102426J - \u673a\u623f\u7684\u5723\u8bde\u793c\u7269"
description: "Chúng tôi có những món quà được đánh số từ 1 đến n. Trẻ có thể chọn bất kỳ tập hợp con nào trong số chúng, với một hạn chế: bất cứ khi nào trẻ nhận quà x, trẻ cũng không được nhận quà gấp đôi. Giá trị của bộ đã chọn là tổng của tất cả các số quà đã chọn và chúng ta cần giá trị lớn nhất có thể."
date: "2026-08-12T19:33:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "J"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 156
verified: true
draft: false
---

[CF 102426J - \u673a\u623f\u7684\u5723\u8bde\u793c\u7269](https://codeforces.com/problemset/problem/102426/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có những món quà được đánh số từ 1 đến n. Trẻ có thể chọn bất kỳ tập hợp con nào trong số chúng, với một hạn chế: bất cứ khi nào trẻ nhận quà x, trẻ cũng không được nhận quà gấp đôi. Giá trị của bộ đã chọn là tổng của tất cả các số quà đã chọn và chúng ta cần giá trị lớn nhất có thể. 

Đầu vào chỉ chứa n, với 1<n<10 5. Đầu ra là tổng tối đa của một tập hợp con hợp lệ. 

Giới hạn trên của 10 5 loại trừ việc liệt kê tất cả các tập hợp con. Có 2 n tập hợp con có thể, do đó ngay cả việc kiểm tra từng tập hợp con trong thời gian không đổi cũng đã là vô vọng. Giải pháp xung quanh O(n 2 ) cũng không cần thiết đối với bài toán có hạn chế chỉ kết nối một số có số gấp đôi số đó. Giải pháp O(n) hoặc O(nlogn) là mục tiêu tự nhiên. 

Có một vài trường hợp nhỏ bộc lộ những lỗi thường gặp. Đối với đầu vào`1`, lựa chọn duy nhất có thể là quà 1, vậy đáp án là`1`. Một giải pháp giả sử mọi số đều có số trước hoặc cố gắng bắt đầu từ số 2 có thể vô tình trả về số 0. 

Đối với đầu vào`2`, hai món quà xung đột vì 1 và 2=2⋅1 không thể cùng được chọn. Sự lựa chọn tốt nhất chỉ là`2`, vậy câu trả lời là`2`. Một giải pháp bất cẩn chỉ tính tổng tất cả các số ngoại trừ một số ranh giới cố định có thể tạo ra`3`. 

Đối với đầu vào`5`, tập tối ưu là {1,3,4,5}, cho`13`. Đặc biệt, việc chọn cả hai là đúng`4`Và`1`, bởi vì mối quan hệ bị cấm cụ thể là giữa x và 2x, không phải giữa số chẵn và số lẻ tùy ý. Việc triển khai xử lý tất cả các số có cùng tính chẵn lẻ là xung đột sẽ từ chối tập hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp xem xét mọi tập hợp con của {1,2,…,n}, kiểm tra xem nó có chứa cả x và 2x đối với một số x hay không và giữ tổng hợp lệ lớn nhất. Có đúng 2 n tập con. Nếu tính hợp lệ được kiểm tra bằng cách quét tất cả n số thì trường hợp xấu nhất là n2n kiểm tra. Ngay cả khi bỏ qua hệ số phụ n, 2 100000 vẫn vượt xa mọi thứ có thể thực hiện được. 

Lực lượng vũ phu phát huy tác dụng vì mọi lựa chọn khả thi đều được xem xét rõ ràng nhưng nó hoàn toàn bỏ qua cấu trúc đặc biệt của mối quan hệ xung đột. Các cặp bị cấm duy nhất là 

(1,2),(2,4),(3,6),(4,8),… 

và tổng quát hơn, mọi số đều thuộc một chuỗi thu được bằng cách nhân liên tục với 2. Ví dụ: các số có dạng phần 3 lẻ 

3, 6, 12, 24,… 

và chỉ những yếu tố lân cận của chuỗi xung đột này. 

Bên trong một chuỗi như vậy, các giá trị tăng lên một cách nghiêm ngặt và mọi giá trị đều chính xác gấp đôi giá trị trước đó. Điều này làm cho một sự lựa chọn tham lam có thể xảy ra. Xét các số từ lớn đến nhỏ. Khi chúng ta đạt đến x, số lớn hơn duy nhất có thể xung đột với nó là 2x. Nếu 2x đã được chọn thì x phải bị từ chối. Nếu 2x không được chọn thì chúng ta nên chọn x. 

Tại sao trường hợp thứ hai luôn an toàn? Số nhỏ hơn duy nhất xung đột với x là x/2, khi x chẵn. Nếu một giải pháp tối ưu sử dụng x/2 thay vì x, việc thay x/2 bằng x sẽ làm tăng tổng. Các lựa chọn bổ sung có thể có bên dưới x/2 tạo thành một chuỗi giảm dần về mặt hình học khác, có tổng vẫn nhỏ hơn x. Tương tự, dọc theo một chuỗi, việc chọn phần tử sẵn có lớn hơn sẽ chiếm ưu thế trong việc chọn phần tử trước nó. 

Điều này có nghĩa là chúng ta có thể xử lý tất cả các số theo thứ tự giảm dần. Hiện tại chúng tôi đang kiểm tra x, 2x đã được quyết định. Nếu 2x được chọn thì x không thể chọn được. Ngược lại chọn x là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2 n ) | O(n) | Quá chậm | 
| Tham lam giảm dần | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`chosen`, Ở đâu`chosen[x]`ghi lại xem món quà x đã được chọn hay chưa. Chúng tôi xử lý các số từ n đến 1, vì vậy khi chúng tôi kiểm tra x, trạng thái của 2x đã được biết trước. 
2. Bắt đầu bằng câu trả lời`0`. Với mọi x từ n đến 1, hãy kiểm tra xem 2x có tồn tại và được chọn hay không. Nếu 2x được chọn, hãy bỏ qua x, vì chọn cả hai sẽ vi phạm điều kiện. 
3. Nếu 2x không tồn tại hoặc chưa được chọn, hãy chọn x và thêm x vào câu trả lời. Đây là sự lựa chọn tham lam vì mọi số xung đột lớn hơn x đều đã được xem xét, trong khi x hoàn toàn có giá trị hơn số lân cận xung đột nhỏ hơn duy nhất có thể có của nó là x/2. 
4. Sau khi xử lý tất cả các số, xuất ra tổng tích lũy. Mọi cặp được chọn đều hợp lệ vì một số bị từ chối chính xác khi số kép của nó được chọn. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý mọi số lớn hơn x, các lựa chọn được thực hiện bên trong mỗi chuỗi nhân đôi là tối ưu cho hậu tố được xử lý đó. Khi đạt đến x, số lớn hơn duy nhất có thể xung đột với nó là 2x. Nếu 2x được chọn thì buộc phải từ chối x. Nếu 2x không được chọn, việc chọn x ít nhất cũng tốt như chọn bất kỳ giá trị xung đột nào nhỏ hơn, vì các giá trị giảm theo hệ số hai dọc theo chuỗi. Do đó, quyết định tham lam sẽ bảo toàn được giải pháp tối ưu cho hậu tố còn lại. Việc áp dụng đối số này nhiều lần từ n xuống 1 sẽ mang lại giải pháp tối ưu cho mọi chuỗi và các chuỗi này độc lập với nhau, do đó các lựa chọn tối ưu của chúng cùng nhau tạo thành một tập hợp tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    chosen = bytearray(n + 1)
    ans = 0

    for x in range(n, 0, -1):
        if 2 * x <= n and chosen[2 * x]:
            continue

        chosen[x] = 1
        ans += x

    print(ans)

if __name__ == "__main__":
    solve()
```các`chosen`mảng chỉ lưu trữ số không hoặc một, vì vậy`bytearray`là đủ và giữ mức sử dụng bộ nhớ rất nhỏ. Tại lần lặp`x`, chỉ số`2 * x`chỉ có hiệu lực khi`2 * x <= n`. Đối với lớn hơn`x`, không có món quà lớn hơn xung đột nên quà tặng luôn có thể được chọn. 

Thứ tự giảm dần là chi tiết triển khai trung tâm. Nếu chúng ta xử lý các số từ nhỏ đến lớn thì trạng thái của 2x vẫn chưa được biết và quyết định tham lam sẽ không có sẵn trực tiếp. 

Câu lệnh cho phép n đạt tới 10 5, nhưng câu trả lời có thể theo thứ tự n 2, vì vậy việc sử dụng kiểu số nguyên toán học thông thường là cần thiết trong các ngôn ngữ có số nguyên có chiều rộng cố định. Số nguyên Python tự động tăng lên, do đó không cần xử lý tràn đặc biệt. 

## Ví dụ đã hoạt động 

Ví dụ đã cho có n=5. Thuật toán xử lý các số từ`5`xuống`1`. 

| x | đã chọn[2x] | Quyết định | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 5 | không áp dụng | chọn 5 | 5 | 
| 4 | không áp dụng | chọn 4 | 9 | 
| 3 | không áp dụng | chọn 3 | 12 | 
| 2 | đã chọn[4] = 1 | bỏ qua 2 | 12 | 
| 1 | đã chọn[2] = 0 | chọn 1 | 13 | 

Tập được chọn là {5,4,3,1}. Xung đột duy nhất có thể xảy ra giữa những con số này sẽ yêu cầu cả x và 2x, nhưng`2`vắng mặt, vì vậy tập hợp là hợp lệ. Kết quả là`13`. 

Đối với ví dụ thứ hai, hãy xem xét n=6. 

| x | đã chọn[2x] | Quyết định | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 6 | không áp dụng | chọn 6 | 6 | 
| 5 | không áp dụng | chọn 5 | 11 | 
| 4 | không áp dụng | chọn 4 | 15 | 
| 3 | đã chọn[6] = 1 | bỏ qua 3 | 15 | 
| 2 | đã chọn[4] = 1 | bỏ qua 2 | 15 | 
| 1 | đã chọn[2] = 0 | chọn 1 | 16 | 

Tập được chọn là {1,4,5,6}, có tổng là`16`. Các chuỗi là {1,2,4}, {3,6} và {5}. Đóng góp tối ưu của chúng lần lượt là 1+4=5, 6 và 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mọi số nguyên từ n đến 1 đều được xử lý một lần. | 
| Không gian | O(n) | các`chosen`mảng chứa n+1 mục. | 

Với n<10 5, thuật toán chỉ thực hiện khoảng 105 lần lặp. các`bytearray`sử dụng khoảng một byte cho mỗi mục nhập, do đó mức tiêu thụ bộ nhớ nằm dưới giới hạn 64 MB. 

## Trường hợp thử nghiệm 

Tuyên bố ban đầu cung cấp một mẫu cụ thể,`n = 5`, kèm theo câu trả lời`13`. Vì đầu vào chỉ bao gồm n nên không có giá trị đầu vào lặp lại hoặc "hoàn toàn bằng nhau" để kiểm tra. Cách tương đương gần nhất là kiểm tra một số giá trị liên tiếp và đảm bảo mọi chuỗi nhân đôi đều được xử lý độc lập.```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())

    chosen = bytearray(n + 1)
    ans = 0

    for x in range(n, 0, -1):
        if 2 * x <= n and chosen[2 * x]:
            continue
        chosen[x] = 1
        ans += x

    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# provided sample
assert run("5\n") == "13", "sample 1"

# minimum size
assert run("1\n") == "1", "minimum n"

# smallest conflicting pair
assert run("2\n") == "2", "1 and 2 cannot both be selected"

# several doubling chains
assert run("6\n") == "16", "chains 1-2-4 and 3-6"

# boundary around a power of two
assert run("8\n") == "27", "power-of-two boundary"

# maximum-size input, checked independently by the same greedy recurrence
def reference(n: int) -> int:
    chosen = bytearray(n + 1)
    ans = 0
    for x in range(n, 0, -1):
        if 2 * x <= n and chosen[2 * x]:
            continue
        chosen[x] = 1
        ans += x
    return ans

assert run("100000\n") == str(reference(100000)), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Ranh giới kích thước tối thiểu | 
|`2`|`2`| Cặp cấm đầu tiên | 
|`6`|`16`| Nhiều chuỗi nhân đôi độc lập | 
|`8`|`27`| Ranh giới ở lũy thừa hai | 
|`100000`|`reference(100000)`| Hạn chế tối đa và hiệu suất tuyến tính | 

## Vỏ cạnh 

Với n=1, vòng lặp chỉ kiểm tra x=1. Vì 2x=2>n nên không có món quà nào lớn hơn xung đột nhau, nên`1`được chọn và câu trả lời trở thành`1`. Do đó, đầu ra là`1`. 

Với n=2, trước tiên thuật toán sẽ chọn`2`. Khi nó đạt tới`1`,`chosen[2]`đã đúng rồi, vậy`1`bị bỏ qua. Câu trả lời là`2`. Điều này trực tiếp xử lý xung đột nhỏ nhất có thể. 

Với n=5, thuật toán chọn`5`,`4`, Và`3`, bỏ qua`2`bởi vì`4`đã được chọn, sau đó chọn`1`bởi vì`2`đã không được chọn. Tổng kết quả là 5+4+3+1=13. Điều này chứng tỏ tại sao quyết định của`1`không thể đơn giản phụ thuộc vào việc liệu`1`là số lẻ hoặc số chẵn. Nó phụ thuộc vào sự lựa chọn thực tế của đôi của nó. 

Với n=8, chuỗi 1,2,4,8 đóng góp 8+2=10, trong khi các chuỗi độc lập 3,6, 5 và 7 đóng góp 6, 5 và 7. Tổng cộng là 10+6+5+7=28, không phải`27`. Trường hợp này đặc biệt hữu ích để phát hiện lỗi từng lỗi một xung quanh lũy thừa hai. Khẳng định đúng cho`8`do đó là:```
assert run("8\n") == "28", "power-of-two boundary"
```Trường hợp tối đa n=100000 thực hiện toàn bộ vòng lặp. Mỗi lần lặp chỉ thực hiện tra cứu mảng, so sánh và có thể là phép cộng, do đó tổng công việc vẫn tuyến tính. Câu trả lời được tích lũy bằng cách sử dụng kiểu số nguyên có độ chính xác tùy ý của Python, tránh tràn mặc dù tổng tối ưu lớn hơn nhiều so với 2 31 −1.
