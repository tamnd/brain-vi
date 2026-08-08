---
title: "CF 102501B - Đa dạng sinh học"
description: "Đầu vào mô tả một cuộc điều tra dân số về động vật trong một khu vườn. Mỗi dòng sau dòng đầu tiên chứa tên của một loài. Nhiệm vụ là tìm hiểu xem liệu một loài có quần thể lớn hơn quần thể của mọi loài khác cộng lại hay không."
date: "2026-08-06T04:42:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102501
codeforces_index: "B"
codeforces_contest_name: "2019-2020 ICPC Southwestern European Regional Programming Contest (SWERC 2019-20)"
rating: 0
weight: 102501
solve_time_s: 65
verified: true
draft: false
---

[CF 102501B - Đa dạng sinh học](https://codeforces.com/problemset/problem/102501/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một cuộc điều tra dân số về động vật trong một khu vườn. Mỗi dòng sau dòng đầu tiên chứa tên của một loài. Nhiệm vụ là tìm hiểu xem liệu một loài có quần thể lớn hơn quần thể của mọi loài khác cộng lại hay không. Nếu một loài như vậy tồn tại, chúng tôi sẽ in tên của nó. Ngược lại, chúng tôi in`NONE`. 

Điều kiện có thể được viết lại bằng cách sử dụng số đếm. Nếu một loài xuất hiện`x`lần trong số`N`động vật, tất cả các loài khác cùng xuất hiện`N - x`lần. Chúng ta cần một loài trong đó`x > N - x`, tương đương với`2x > N`. Điều này có nghĩa là câu trả lời phải là một loài đa số nghiêm ngặt. 

Giới hạn của`N`đang lên đến`2 * 10^5`có nghĩa là mỗi con vật chỉ có thể được xử lý một số lần nhỏ. Một cách tiếp cận so sánh mọi loài với mọi loài khác có thể đòi hỏi khoảng`N^2`hoạt động, quá nhiều ở quy mô này. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Một số trường hợp rất dễ xử lý sai. Nếu loài lớn nhất xuất hiện đúng bằng một nửa số loài thì vẫn chưa đủ vì yêu cầu rất khắt khe. Ví dụ:```
4
cat
dog
cat
dog
```Đầu ra đúng là:```
NONE
```Một giải pháp bất cẩn kiểm tra`>=`thay vì`>`sẽ chọn sai`cat`hoặc`dog`. 

Một trường hợp khó khăn khác là khi chỉ có một con vật. Loài duy nhất xuất hiện nhiều lần hơn tất cả các loài khác cộng lại vì phía bên kia có kích thước bằng 0.```
1
lion
```Đầu ra đúng là:```
lion
```Một giải pháp giả định phải có ít nhất hai loài khác nhau có thể thất bại ở đây. 

Một trường hợp khó phát hiện cuối cùng là khi một số loài tồn tại nhưng một trong số chúng hầu như không vượt qua ngưỡng.```
5
bird
bird
bird
fish
frog
```Đầu ra đúng là:```
bird
```Số lượng là ba, trong khi mọi loài khác cộng lại đều có hai con vật. Các giải pháp chỉ tìm kiếm các loài phổ biến nhất mà không kiểm tra điều kiện đa số có thể tạo ra câu trả lời sai trong trường hợp số lượng tối đa vẫn không đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là đếm từng loài và sau đó kiểm tra từng loài riêng biệt. Sau khi xây dựng số lượng, chúng ta có thể lấy từng loài và so sánh số lượng của nó với tổng của tất cả các số lượng khác. Phương thức này đúng vì nó kiểm tra điều kiện chính xác từ câu lệnh. Tuy nhiên, nếu thực hiện kém bằng cách quét tất cả các loài để tìm mọi ứng cử viên, số lượng so sánh có thể đạt tới bình phương số lượng động vật. Với`N = 200000`, điều này có thể đạt tới bốn mươi tỷ phép tính, điều này là không khả thi. 

Quan sát hữu ích là điều kiện chỉ phụ thuộc vào số lượng lớn nhất. Nếu một số loài có nhiều động vật hơn tất cả các loài khác cộng lại thì loài đó cũng phải là loài thường xuyên nhất. Chúng ta chỉ cần biết loài nào có tần số tối đa và tần số của nó có lớn hơn`N / 2`. 

Điều này cho phép một giải pháp hai bước đơn giản. Đầu tiên, hãy đếm xem mỗi loài xuất hiện bao nhiêu lần. Thứ hai, tìm kiếm một loài có số lượng thỏa mãn`2 * count > N`. Từ điển xử lý việc đếm một cách hiệu quả và tổng công việc tỷ lệ thuận với số lượng động vật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(N) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả tên loài và đếm số lần xuất hiện của chúng bằng bản đồ băm. Mỗi loài động vật đóng góp chính xác một số gia tăng vào bộ đếm loài của nó, vì vậy bước này thể hiện sự phân bố quần thể hoàn chỉnh. 
2. Lưu trữ tổng số vật nuôi dưới dạng`N`. Điều kiện đa số cho một loài có tần số`c`là`c > N - c`, điều này cũng giống như`2 * c > N`. 
3. Lặp lại bản đồ tần số và tìm bất kỳ loài nào thỏa mãn`2 * count > N`. Vì hai loài khác nhau không thể cùng có hơn một nửa tổng quần thể nên chỉ cần tìm một loài hợp lệ là đủ. 
4. Nếu không có loài nào thỏa mãn điều kiện thì xuất ra`NONE`. 

Tại sao nó hoạt động: loài được yêu cầu phải có số lượng lớn hơn mọi loài khác cộng lại. Điều đó có nghĩa là số lượng của nó phải lớn hơn một nửa số động vật. Thuật toán kiểm tra chính xác thuộc tính này cho mọi loài sau khi tính toán tần số chính xác. Bất kỳ câu trả lời hợp lệ nào cũng sẽ vượt qua điều kiện và không có loài không hợp lệ nào có thể vượt qua nó vì sự bất đẳng thức chính xác là định nghĩa của đa số bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return

    n = int(n_line)
    count = {}

    for _ in range(n):
        species = input().strip()
        count[species] = count.get(species, 0) + 1

    for species, freq in count.items():
        if freq * 2 > n:
            print(species)
            return

    print("NONE")

if __name__ == "__main__":
    solve()
```Từ điển lưu trữ số lần xuất hiện của mỗi loài. Từ điển Python cung cấp khả năng chèn và tra cứu theo thời gian cố định trung bình, do đó việc xử lý tất cả các dòng đầu vào vẫn tuyến tính. 

Vòng cuối cùng không cần so sánh với các loài khác. biểu hiện`freq * 2 > n`đã thể hiện liệu loài này có lớn hơn tất cả các loài động vật còn lại cộng lại hay không. Phép nhân được sử dụng thay vì phép chia để tránh mọi vấn đề về ranh giới khi chia số nguyên. 

Không có vấn đề tràn số nguyên trong Python vì số nguyên tự động tăng khi cần. Việc đọc đầu vào sử dụng`sys.stdin.readline`, phù hợp với số lượng lớn các dòng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
frog
fish
frog
```Quá trình đếm là: 

| Đọc động vật | Số lượng hiện tại | Kiểm tra | 
| --- | --- | --- | 
| ếch | ếch: 1 | tiếp tục | 
| cá | ếch: 1, cá: 1 | tiếp tục | 
| ếch | ếch: 2, cá: 1 | tiếp tục | 

Số đếm cuối cùng là`frog = 2`Và`fish = 1`. Từ`2 * 2 > 3`,`frog`là một câu trả lời hợp lệ 

Đối với mẫu thứ hai:```
4
cat
mouse
mouse
cat
```Quá trình đếm là: 

| Đọc động vật | Số lượng hiện tại | Kiểm tra | 
| --- | --- | --- | 
| mèo | mèo: 1 | tiếp tục | 
| chuột | mèo: 1, chuột: 1 | tiếp tục | 
| chuột | mèo: 1, chuột: 2 | tiếp tục | 
| mèo | mèo: 2, chuột: 2 | tiếp tục | 

Cả hai loài đều có tần số`2`. Bài kiểm tra đa số cho`2 * 2 > 4`, đó là sai, vì vậy câu trả lời là`NONE`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi tên loài được đọc một lần và mỗi tần số lưu trữ được kiểm tra một lần. | 
| Không gian | O(N) | Trong trường hợp xấu nhất, mỗi loài động vật có một tên loài khác nhau nên từ điển lưu trữ N mục. | 

Kích thước đầu vào tối đa là`200000`động vật. Một đường chuyền tuyến tính dễ dàng phù hợp với giới hạn thời gian sẵn có và kích thước từ điển nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.readline
    n = int(data())
    count = {}

    for _ in range(n):
        s = data().strip()
        count[s] = count.get(s, 0) + 1

    ans = "NONE"
    for s, c in count.items():
        if c * 2 > n:
            ans = s
            break

    sys.stdin = old_stdin
    return ans

assert solution("3\nfrog\nfish\nfrog\n") == "frog", "sample 1"
assert solution("4\ncat\nmouse\nmouse\ncat\n") == "NONE", "sample 2"

assert solution("1\nlion\n") == "lion", "single animal"
assert solution("5\na\na\na\nb\nc\n") == "a", "bare majority"
assert solution("6\nx\nx\ny\ny\nz\nz\n") == "NONE", "exact half"
assert solution("200000\n" + "bird\n" * 200000) == "bird", "maximum size all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`động vật cùng một loài |`lion`| Xử lý đầu vào nhỏ nhất có thể. | 
| Ba bản sao của một loài trong số năm loài động vật |`a`| Kiểm tra đa số nghiêm ngặt ngay phía trên ranh giới. | 
| Ba loài có số lượng bằng nhau |`NONE`| Kiểm tra chính xác một nửa điều kiện và ngăn chặn`>=`sai lầm. | 
| Hai trăm ngàn loài giống hệt nhau |`bird`| Xác nhận phương pháp tuyến tính xử lý kích thước đầu vào tối đa. | 

## Vỏ cạnh 

Trường hợp nửa chính xác được xử lý bằng phép kiểm nhân. Vì:```
4
cat
dog
cat
dog
```từ điển chứa`cat: 2`Và`dog: 2`. Thuật toán kiểm tra`2 * 2 > 4`, điều này sai cho cả hai, vì vậy nó in`NONE`. 

Trường hợp một con vật hoạt động vì tần số duy nhất là`1`, và điều kiện trở thành`2 * 1 > 1`, đó là sự thật. Thuật toán in các loài duy nhất mà không cần xử lý đặc biệt. 

Trường hợp hầu như không chiếm đa số:```
5
bird
bird
bird
fish
frog
```sản xuất`bird: 3`,`fish: 1`, Và`frog: 1`. Séc`2 * 3 > 5`thành công, vậy`bird`được trả lại. Thuật toán không cần biết tổng chính xác của các loài khác vì sự bất bình đẳng đa số đã nắm bắt được nó. 

Tôi cũng có thể điều chỉnh bài xã luận này thành một phiên bản ngắn hơn theo phong cách Codeforces với ít trình bày hơn và bằng chứng nhỏ gọn hơn nếu bạn cần.
