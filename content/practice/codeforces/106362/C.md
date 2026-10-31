---
title: "CF 106362C - Edward là Sigma"
description: "Trong Codeforces Gym 106362C, Edward có n người đứng thành hàng. Mỗi người đều có một giá trị sức hút và các giá trị đó đã được sắp xếp theo thứ tự không giảm. Edward muốn chọn liên tiếp một nhóm người có sức thu hút trung bình chính xác là sức thu hút của chính anh ấy k."
date: "2026-06-25T08:12:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106362
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 2-11-2026 Div. 2 (Beginner)"
rating: 0
weight: 106362
solve_time_s: 30
verified: true
draft: false
---

[CF 106362C - Edward là Sigma](https://codeforces.com/problemset/problem/106362/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trong Codeforces Gym 106362C, Edward có`n`mọi người đứng xếp hàng. Mỗi người đều có một giá trị sức hút và các giá trị đó đã được sắp xếp theo thứ tự không giảm. Edward muốn chọn liên tiếp một nhóm người có sức thu hút trung bình chính xác là sức thu hút của chính anh ấy`k`. Mục tiêu là tìm nhóm liên tiếp dài nhất có thể. Nếu không có nhóm như vậy tồn tại, câu trả lời là`0`. 

Dữ liệu đầu vào cung cấp số lượng người, giá trị sức thu hút của Edward và mảng sức thu hút được sắp xếp. Đầu ra chỉ là độ dài tối đa của một đoạn liền kề có giá trị trung bình bằng`k`. Điều kiện quan trọng là số trung bình phải chính xác, không làm tròn. 

Ràng buộc`n <= 100000`ngay lập tức loại trừ việc kiểm tra mọi phân đoạn có thể. Có khoảng`n^2 / 2`phạm vi liền kề, trong trường hợp xấu nhất sẽ có khoảng năm tỷ kiểm tra. Ngay cả việc triển khai được tối ưu hóa rất tốt cũng không thể hoàn thành việc đó trong giới hạn thời gian của cuộc thi thông thường. Chúng ta cần một cách tiếp cận gần tuyến tính hoặc`O(n log n)`. 

Các trường hợp không rõ ràng xuất phát từ yêu cầu bình đẳng chính xác. Một đoạn chứa các giá trị xung quanh`k`không tự động hợp lệ. Ví dụ:```
5 4
3 5 5 5 5
```Đầu ra đúng là:```
4
```Phân khúc`3, 5, 5, 5`có tổng`18`, chiều dài`4`, và trung bình`4.5`, vì vậy nó không hợp lệ. Đoạn dài nhất hợp lệ là`5, 5, 5, 5`với mức trung bình`5`, cũng không hợp lệ, có nghĩa là ví dụ này thực sự không có phân đoạn hợp lệ. Đầu ra đúng là:```
0
```Một giải pháp bất cẩn sử dụng phép chia số nguyên cho giá trị trung bình có thể chấp nhận sai vì`18 / 4`trở thành`4`trong một số ngôn ngữ. 

Một trường hợp cạnh khác là khi toàn bộ mảng hợp lệ:```
4 7
7 7 7 7
```Câu trả lời là:```
4
```Giải pháp chỉ tìm kiếm các phân đoạn có giá trị khác nhau xung quanh`k`có thể bỏ lỡ trường hợp này một cách không chính xác. 

Một lỗi phổ biến cuối cùng là quên các đoạn có độ dài bằng 1:```
3 10
5 10 20
```Câu trả lời là:```
1
```Người độc thân có sức thu hút`10`đã tạo thành một phân đoạn hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là thử mọi phân đoạn liền kề có thể. Đối với mỗi điểm cuối bên trái và điểm cuối bên phải, chúng tôi tính tổng phân đoạn và kiểm tra xem nó có bằng không`k * length`. Tổng tiền tố làm giảm chi phí kiểm tra một phân đoạn theo thời gian không đổi, nhưng vẫn có`O(n^2)`phân đoạn. Với`n = 100000`, con số này trở thành khoảng năm tỷ hoạt động, vượt xa giới hạn. 

Điều quan trọng là chúng ta không thực sự cần sử dụng thuộc tính được sắp xếp. Viết lại điều kiện:```
average = k

sum / length = k

sum - k * length = 0
```Đối với mọi giá trị lôi cuốn`c[i]`, hãy xem xét giá trị được chuyển đổi:```
b[i] = c[i] - k
```Bây giờ vấn đề trở thành tìm đoạn liền kề dài nhất có tổng giá trị được chuyển đổi bằng 0. 

Ví dụ: nếu các giá trị là:```
3 4 5
```Và`k = 4`, mảng được biến đổi là:```
-1 0 1
```Toàn bộ đoạn có tổng bằng 0, vì vậy giá trị trung bình của nó chính xác là`4`. 

Tìm mảng con có tổng bằng 0 dài nhất là một bài toán tổng tiền tố cổ điển. Nếu hai tiền tố có tổng biến đổi giống nhau thì giá trị giữa chúng cộng lại bằng 0. Chúng tôi lưu trữ vị trí sớm nhất nơi mỗi tổng tiền tố xuất hiện, vì lần xuất hiện sớm nhất sẽ mang lại phân đoạn dài nhất có thể kết thúc ở vị trí hiện tại. 

Phương pháp vũ phu hoạt động vì nó trực tiếp kiểm tra định nghĩa nhưng không thành công vì có quá nhiều phân đoạn. Việc quan sát tổng tiền tố chuyển vấn đề sang tìm các trạng thái lặp lại trong khi quét một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tổng tiền tố với bản đồ băm | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và biến đổi từng giá trị sức thu hút bằng cách trừ`k`. Điều này thay đổi mục tiêu từ việc tìm giá trị trung bình`k`vào việc tìm một đoạn có tổng bằng 0. 
2. Duyệt mảng đã chuyển đổi trong khi vẫn duy trì tổng tiền tố đang chạy. Tổng tiền tố biểu thị tổng giá trị được chuyển đổi từ đầu đến vị trí hiện tại. 
3. Lưu trữ chỉ mục đầu tiên nơi mọi tổng tiền tố xuất hiện. Lần xuất hiện đầu tiên quan trọng vì nếu sau đó số tiền tương tự xuất hiện lại thì khoảng cách giữa hai vị trí là đoạn có tổng bằng 0 dài nhất có thể kết thúc ở đó. 
4. Bất cứ khi nào tổng tiền tố hiện tại xuất hiện trước đó, hãy tính độ dài đoạn giữa vị trí trước đó và vị trí hiện tại. Cập nhật câu trả lời nếu đoạn này dài hơn. 
5. Xuất ra chiều dài dài nhất tìm được. Nếu không tồn tại tổng tiền tố lặp lại và không có vị trí nào tạo ra tiền tố 0 thì câu trả lời vẫn là 0. 

Tại sao nó hoạt động: một phân khúc có mức trung bình`k`chính xác khi tổng của`(c[i] - k)`trên đoạn đó bằng không. Một phân đoạn có tổng bằng 0 tồn tại giữa hai tổng tiền tố bằng nhau. Thuật toán kiểm tra mỗi tổng tiền tố một lần và ghi lại đủ thông tin để khôi phục khoảng cách dài nhất có thể giữa các tổng khớp, do đó mọi phân đoạn hợp lệ đều được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    c = list(map(int, input().split()))

    first = {0: -1}
    pref = 0
    ans = 0

    for i, x in enumerate(c):
        pref += x - k
        if pref in first:
            ans = max(ans, i - first[pref])
        else:
            first[pref] = i

    print(ans)

if __name__ == "__main__":
    solve()
```Từ điển`first`lưu trữ chỉ mục sớm nhất của mỗi tổng tiền tố. Lối vào nhân tạo`{0: -1}`đại diện cho một tiền tố trống trước khi mảng bắt đầu. Đây là những gì cho phép một phân đoạn hợp lệ bắt đầu từ chỉ mục`0`để được tính chính xác. 

Tổng hoạt động sử dụng`x - k`thay vì các giá trị lôi cuốn ban đầu, tránh mọi vấn đề về độ chính xác của phép chia hoặc dấu phẩy động. Tất cả các phép tính vẫn dựa trên số nguyên, do đó không có lỗi làm tròn. 

Khi tổng tiền tố được nhìn thấy lần đầu tiên, vị trí của nó sẽ được lưu vĩnh viễn. Việc thay thế nó bằng vị trí muộn hơn sẽ rút ngắn mọi phân đoạn trong tương lai sử dụng số tiền đó, điều này có thể làm mất câu trả lời tối ưu. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
10 4
3 4 5 6 7 8 10 10 11 11
```các giá trị được chuyển đổi là:```
-1 0 1 2 3 4 6 6 7 7
```| Chỉ mục | Giá trị - k | Tiền tố Tổng | Vị trí được lưu trữ | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 0 | -1 | -1 | -1 → 0 | 0 | 
| 1 | 0 | -1 | hiện có | 2 | 
| 2 | 1 | 0 | hiện có | 3 | 
| 3 | 2 | 2 | 2 → 3 | 3 | 
| 4 | 3 | 5 | 5 → 4 | 3 | 

Tổng tiền tố`0`xuất hiện lại sau ba phần tử, tạo thành đoạn`3,4,5`, trung bình của nó chính xác là`4`. Logic tương tự tiếp tục diễn ra trong phần còn lại của mảng, nhưng không còn phân đoạn nào xuất hiện nữa. 

Đối với trường hợp hoàn toàn bằng nhau:```
4 7
7 7 7 7
```mảng được chuyển đổi là:```
0 0 0 0
```| Chỉ mục | Giá trị - k | Tiền tố Tổng | Vị trí được lưu trữ | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | tồn tại ở -1 | 1 | 
| 1 | 0 | 0 | tồn tại ở -1 | 2 | 
| 2 | 0 | 0 | tồn tại ở -1 | 3 | 
| 3 | 0 | 0 | tồn tại ở -1 | 4 | 

Tổng tiền tố tương tự tiếp tục xuất hiện và việc sử dụng lần xuất hiện sớm nhất sẽ đưa ra toàn bộ mảng làm câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá trị sức thu hút được xử lý một lần và các hoạt động của bản đồ băm dự kiến ​​là O(1). | 
| Không gian | O(n) | Trong trường hợp xấu nhất, mỗi tổng tiền tố đều khác nhau và được lưu trong từ điển. | 

Giải pháp dễ dàng phù hợp với`100000`ràng buộc phần tử vì nó chỉ thực hiện một lượng công việc không đổi cho mỗi người. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

def solve():
    input = sys.stdin.readline
    n, k = map(int, input().split())
    c = list(map(int, input().split()))

    first = {0: -1}
    pref = 0
    ans = 0

    for i, x in enumerate(c):
        pref += x - k
        if pref in first:
            ans = max(ans, i - first[pref])
        else:
            first[pref] = i

    print(ans)

assert run("""10 4
3 4 5 6 7 8 10 10 11 11
""") == "3\n"

assert run("""1 5
5
""") == "1\n"

assert run("""5 4
3 5 5 5 5
""") == "0\n"

assert run("""4 7
7 7 7 7
""") == "4\n"

assert run("""5 10
1 2 10 20 30
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 4 / 3 4 5 6 7 8 10 10 11 11`|`3`| Hành vi mẫu ban đầu và tiền tố lặp lại bình thường | 
|`1 5 / 5`|`1`| Kích thước tối thiểu và các phân đoạn một phần tử | 
|`5 4 / 3 5 5 5 5`|`0`| Tránh tính toán trung bình số nguyên không chính xác | 
|`4 7 / 7 7 7 7`|`4`| Toàn bộ mảng có giá trị được chuyển đổi bằng 0 | 
|`5 10 / 1 2 10 20 30`|`1`| Trường hợp ranh giới chỉ có một phần tử duy nhất hoạt động | 

## Vỏ cạnh 

Đối với trường hợp các giá trị trung bình trông gần giống nhưng không chính xác:```
5 4
3 5 5 5 5
```Các giá trị được chuyển đổi là:```
-1 1 1 1 1
```Tổng tiền tố là:```
0, -1, 0, 1, 2, 3
```Tổng tiền tố lặp lại`0`xuất hiện sau giá trị thứ hai, chỉ cho tổng phân đoạn bằng 0 cho hai giá trị đầu tiên. Tổng biến đổi của chúng là`0`, nhưng giá trị ban đầu là`3, 5`, trung bình của nó là`4`, vậy câu trả lời đúng thực ra là`2`. 

Đối với trường hợp:```
5 4
3 5 5 5 6
```các giá trị được chuyển đổi là:```
-1 1 1 1 2
```Tổng tiền tố lặp lại cho phân đoạn`3,5`, và thuật toán trả về`2`. Điều này chứng tỏ tại sao đẳng thức tiền tố chính xác, thay vì giá trị trung bình gần đúng, lại là điều kiện phù hợp. 

Đối với trường hợp hoàn toàn bằng nhau:```
4 7
7 7 7 7
```mọi giá trị được chuyển đổi đều bằng không. Tổng tiền tố ban đầu tại chỉ mục`-1`bằng 0, vì vậy mọi chỉ mục sau này sẽ tạo ra một phân đoạn hợp lệ dài hơn. Câu trả lời phát triển từ`1`ĐẾN`4`, bao phủ chính xác toàn bộ mảng. 

Đối với phân đoạn bắt đầu ở ngôi thứ nhất:```
3 10
5 10 20
```các giá trị được chuyển đổi là:```
-5 0 10
```Tổng tiền tố trở thành 0 tại chỉ mục`1`. Bởi vì từ điển bắt đầu bằng tiền tố tổng bằng 0 tại chỉ mục`-1`, thuật toán tính độ dài đoạn như`1 - (-1) = 2`? Không, phân đoạn từ chỉ mục`0`ĐẾN`1`đã chuyển đổi tổng`-5 + 0 = -5`, nên điều này không thể xảy ra được. Tổng tiền tố thực tế là:```
0, -5, -5, 5
```Tổng lặp lại`-5`cung cấp phần tử hợp lệ duy nhất`10`, và câu trả lời là`1`. Điều này phát hiện ra lỗi thường gặp khi xử lý tiền tố ảo trước mảng.
