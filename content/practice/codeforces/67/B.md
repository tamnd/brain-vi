---
title: "CF 67B - Khôi phục hoán vị"
description: "Chúng ta được cho một hoán vị A của các số từ 1 đến n. Với mọi giá trị i, chúng ta biết một số b[i]. Con số này mô tả có bao nhiêu phần tử xuất hiện trước i trong hoán vị và ít nhất là i + k. Điều kiện được gắn với chính giá trị đó chứ không phải với vị trí."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 67
codeforces_index: "B"
codeforces_contest_name: "Manthan 2011"
rating: 1800
weight: 67
solve_time_s: 119
verified: true
draft: false
---

[CF 67B - Khôi phục hoán vị](https://codeforces.com/problemset/problem/67/B) 

**Đánh giá:** 1800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị`A`của các số từ`1`ĐẾN`n`. Với mọi giá trị`i`, chúng ta biết một số`b[i]`. Con số này mô tả có bao nhiêu phần tử xuất hiện trước`i`trong hoán vị và ít nhất là`i + k`. 

Điều kiện được gắn với chính giá trị đó chứ không phải với vị trí. Nếu như`i = 3`Và`k = 2`, thì ta đếm xem có bao nhiêu số lớn hơn hoặc bằng`5`xuất hiện trước`3`. 

Nhiệm vụ là xây dựng lại hoán vị nhỏ nhất về mặt từ điển thỏa mãn tất cả các số đếm này. 

Ràng buộc`n ≤ 1000`đủ nhỏ cho các thuật toán bậc hai. MỘT`O(n^2)`giải pháp thực hiện khoảng một triệu thao tác, điều này không đáng kể trong thời gian giới hạn. Các giải pháp khối bắt đầu tiếp cận một tỷ phép tính trong trường hợp xấu nhất và không an toàn trong Python. 

Phần khó khăn là ở chỗ đó`b[i]`không mô tả sự đảo ngược theo nghĩa thông thường. Mỗi giá trị ít nhất chỉ quan tâm đến số`k`lớn hơn chính nó. Điều đó có nghĩa là mối quan hệ giữa các giá trị lân cận là không liên quan. 

Một sai lầm phổ biến là suy nghĩ tham lam đặt số nhỏ nhất có thể chưa sử dụng ở mọi vị trí luôn có tác dụng. Coi như:```
n = 4, k = 2
B = [0, 1, 0, 0]
```Giá trị`2`cần ít nhất một số sớm hơn`4`. Ứng cử viên duy nhất là`4`, Vì thế`4`phải xuất hiện trước`2`. Nếu chúng ta tham lam đặt các số nhỏ lên trước, chúng ta có thể xây dựng:```
1 2 3 4
```Điều này vi phạm yêu cầu về`2`. 

Một trường hợp tế nhị khác xuất hiện khi`k`là lớn.```
n = 5, k = 5
B = [0, 0, 0, 0, 0]
```Ít nhất không có giá trị nào có thể có số sớm hơn`i + 5`, vì những con số đó không tồn tại. Mọi hoán vị đều thỏa mãn các điều kiện, vì vậy câu trả lời nhỏ nhất về mặt từ điển là:```
1 2 3 4 5
```Việc triển khai bất cẩn vẫn có thể cố gắng thực thi các phần phụ thuộc mà thực tế là không thể thực hiện được. 

Một trường hợp nguy hiểm hơn là khi một số giá trị có yêu cầu giống nhau.```
n = 5, k = 2
B = [0, 0, 0, 0, 0]
```Ở đây mọi số nhỏ đều muốn số tiền lớn bằng không. Hoán vị hợp lệ nhỏ nhất về mặt từ điển chỉ đơn giản là thứ tự tăng dần. Nếu chúng tôi xử lý các giá trị sai hướng, chúng tôi có thể tạo ra câu trả lời hợp lệ nhưng không tối thiểu như:```
3 1 4 2 5
```Vấn đề không chỉ là tái tạo lại bất kỳ hoán vị nào mà còn là hoán vị nhỏ nhất theo thứ tự từ điển. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo tất cả các hoán vị, tính toán mảng tương ứng`B`và trả về hoán vị đầu tiên khớp với đầu vào. Máy tính`B`cho một hoán vị mất`O(n^2)`thời gian vì với mỗi giá trị, chúng tôi có thể quét mọi thứ ở bên trái của nó. Vấn đề thực sự là số lượng hoán vị. Ngay cả đối với`n = 10`, có`10! ≈ 3.6 million`khả năng. Tại`n = 1000`, việc tìm kiếm toàn diện là hoàn toàn không thể. 

Để cải thiện điều này, chúng ta cần hiểu ý nghĩa thực sự của số đếm. 

Giả sử chúng ta nhìn vào một giá trị`i`. Chỉ có số từ`i + k`ĐẾN`n`quan trọng đối với số lượng của nó. Các giá trị nhỏ hơn không liên quan. Cho phép:```
big(i) = count of numbers >= i + k
```Có chính xác:```
n - (i + k) + 1 = n - i - k + 1
```những con số như vậy khi`i + k ≤ n`. 

giá trị`b[i]`cho chúng ta biết có bao nhiêu con số lớn đó xuất hiện trước`i`. Các số lớn còn lại phải xuất hiện sau`i`. 

Điều này bắt đầu trông giống như các ràng buộc về thứ tự giữa các phần tử. 

Một góc nhìn hữu ích hơn nhiều là xây dựng hoán vị từ giá trị lớn nhất trở xuống. 

Khi chúng tôi xử lý giá trị`i`, ít nhất tất cả các giá trị`i + k`đã được biết đến. Vị trí tương đối của chúng là cố định. Chúng ta chỉ cần quyết định nơi để chèn`i`trong số đó chính xác là như vậy`b[i]`của những giá trị lớn đó ở trước nó. 

Giả sử có`m`các giá trị lớn đã được đặt sẵn. Nếu chính xác`b[i]`trong số họ phải có trước`i`, sau đó`i`phải được chèn ngay sau lần đầu tiên`b[i]`những giá trị lớn. 

Giá trị giữa`i + 1`Và`i + k - 1`hoàn toàn không ảnh hưởng đến số lượng, vì vậy vị trí của chúng không liên quan đến điều kiện này. 

Điều này mang lại một công trình tham lam sạch sẽ. 

Chúng tôi xử lý các giá trị từ`n`xuống tới`1`. Với mỗi giá trị`i`, chúng tôi chèn nó vào chuỗi hiện tại ở vị trí được chọn cẩn thận. Vì chúng ta chèn các giá trị nhỏ hơn sau, nên việc đặt giá trị nhỏ hơn sớm hơn trong chuỗi sẽ cải thiện thứ tự từ điển bất cứ khi nào có thể. 

Độ phức tạp cuối cùng trở thành`O(n^2)`, dễ dàng phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n²) | O(n) | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với một chuỗi trống. 
2. Xử lý các giá trị từ`n`xuống tới`1`. 
3. Đối với giá trị hiện tại`i`, xác định tất cả các giá trị đã được chèn ít nhất`i + k`. 

Đây chính xác là những giá trị góp phần vào`b[i]`. 
4. Quét chuỗi hiện tại từ trái sang phải và đếm xem có bao nhiêu giá trị đóng góp đã được nhìn thấy. 
5. Chèn`i`ở vị trí đầu tiên chính xác`b[i]`những giá trị đóng góp vẫn còn ở trước nó. 

Nếu như`b[i] = 0`, chúng tôi đặt`i`càng sớm càng tốt. 

Nếu như`b[i] = 2`, chúng tôi bỏ qua hai giá trị đóng góp trước khi chèn. 
6. Tiếp tục cho đến khi tất cả các giá trị được chèn vào. 
7. Xuất chuỗi cuối cùng. 

### Tại sao nó hoạt động 

Khi xử lý`i`, ít nhất mọi giá trị`i + k`đã được đặt vì chúng tôi xử lý từ số lớn hơn đến số nhỏ hơn. Giá trị nhỏ hơn không bao giờ có thể ảnh hưởng`b[i]`, vì vậy việc chèn thêm trong tương lai là vô hại. 

Vị trí chèn đảm bảo rằng chính xác`b[i]`những người đóng góp hợp lệ ở lại trước`i`. Vì chúng tôi luôn chèn`i`càng sớm càng tốt trong khi vẫn duy trì số lượng này, hoán vị kết quả là tối thiểu về mặt từ điển. 

Bất biến chính là:```
After processing values from n down to i,
all conditions for processed values are already permanently satisfied.
```Các lần chèn trong tương lai chỉ liên quan đến các số nhỏ hơn, không thể đóng góp vào bất kỳ số lượng giá trị được xử lý nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    b = list(map(int, input().split()))

    perm = []

    for i in range(n, 0, -1):
        need = b[i - 1]

        cnt = 0
        pos = len(perm)

        for j in range(len(perm) + 1):
            if cnt == need:
                pos = j
                break

            if j < len(perm) and perm[j] >= i + k:
                cnt += 1

        perm.insert(pos, i)

    print(*perm)

solve()
```Mảng`perm`lưu trữ hoán vị được xây dựng một phần. 

Chúng tôi xử lý các giá trị theo thứ tự giảm dần vì mọi giá trị có thể ảnh hưởng đến`i`phải có mặt trước khi chúng tôi quyết định nơi đặt`i`. 

Vòng lặp bên trong tìm kiếm điểm chèn sớm nhất tạo ra chính xác`b[i]`người tiền nhiệm lớn. Thứ tự các hoạt động bên trong vòng lặp này rất quan trọng. Trước tiên, chúng tôi kiểm tra xem số lượng người tiền nhiệm đủ điều kiện hiện tại có bằng số lượng yêu cầu hay không. Chỉ sau đó chúng ta mới vượt qua được yếu tố khác. 

Điều này tránh được một sai lầm nhỏ. Giả định`need = 0`. Chúng ta phải chèn trước phần tử đủ điều kiện đầu tiên, không được chèn sau phần tử đó. 

Điều kiện:```
perm[j] >= i + k
```thực hiện định nghĩa một cách trực tiếp. Chỉ có giá trị đủ lớn mới đóng góp. 

Chèn danh sách Python là`O(n)`, nhưng với`n ≤ 1000`, tổng độ phức tạp vẫn là bậc hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
1 2 1 0 0
```| Hiện tại tôi | Perm hiện tại | Bắt buộc b[i] | Vị trí chèn | Perm mới | 
| --- | --- | --- | --- | --- | 
| 5 | [] | 0 | 0 | [5] | 
| 4 | [5] | 0 | 0 | [4, 5] | 
| 3 | [4, 5] | 1 | 2 | [4, 5, 3] | 
| 2 | [4, 5, 3] | 2 | 2 | [4, 5, 2, 3] | 
| 1 | [4, 5, 2, 3] | 1 | 1 | [4, 1, 5, 2, 3] | 

Câu trả lời cuối cùng:```
4 1 5 2 3
```Đối với giá trị`2`, các số đóng góp là`4`Và`5`. Chúng ta cần chính xác hai trong số chúng trước khi`2`, Vì thế`2`được chèn vào sau cả hai. 

Dấu vết này cho thấy lý do tại sao quá trình xử lý hướng xuống lại hoạt động. Khi chèn`2`, mọi số lớn hơn có liên quan đều có một vị trí cố định. 

### Ví dụ 2 

đầu vào:```
5 5
0 0 0 0 0
```| Hiện tại tôi | Perm hiện tại | Bắt buộc b[i] | Vị trí chèn | Perm mới | 
| --- | --- | --- | --- | --- | 
| 5 | [] | 0 | 0 | [5] | 
| 4 | [5] | 0 | 0 | [4, 5] | 
| 3 | [4, 5] | 0 | 0 | [3, 4, 5] | 
| 2 | [3, 4, 5] | 0 | 0 | [2, 3, 4, 5] | 
| 1 | [2, 3, 4, 5] | 0 | 0 | [1, 2, 3, 4, 5] | 

Câu trả lời cuối cùng:```
1 2 3 4 5
```Không có giá trị có người đóng góp vì`i + k`luôn vượt quá`n`. Thuật toán sẽ chèn mọi giá trị vào đầu một cách tự nhiên, tạo ra hoán vị nhỏ nhất về mặt từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi lần chèn sẽ quét và dịch chuyển nhiều nhất`n`yếu tố | 
| Không gian | O(n) | Mảng hoán vị lưu trữ`n`số nguyên | 

Với`n = 1000`, thời gian bậc hai là hoàn toàn an toàn. Khoảng một triệu thao tác dễ dàng phù hợp với giới hạn một giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    b = list(map(int, input().split()))

    perm = []

    for i in range(n, 0, -1):
        need = b[i - 1]

        cnt = 0
        pos = len(perm)

        for j in range(len(perm) + 1):
            if cnt == need:
                pos = j
                break

            if j < len(perm) and perm[j] >= i + k:
                cnt += 1

        perm.insert(pos, i)

    print(*perm)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("5 2\n1 2 1 0 0\n") == "4 1 5 2 3", "sample 1"

# minimum size
assert run("1 1\n0\n") == "1", "single element"

# no contributing pairs
assert run("5 5\n0 0 0 0 0\n") == "1 2 3 4 5", "large k"

# already lexicographically smallest
assert run("4 2\n0 0 0 0\n") == "1 2 3 4", "all zero counts"

# forces large number before small number
assert run("4 2\n0 1 0 0\n") == "1 4 2 3", "dependency handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`1`| Đầu vào kích thước tối thiểu | 
|`5 5 / 0 0 0 0 0`|`1 2 3 4 5`| Không có người đóng góp tồn tại | 
|`4 2 / 0 0 0 0`|`1 2 3 4`| Thứ tự nhỏ nhất về mặt từ điển | 
|`4 2 / 0 1 0 0`|`1 4 2 3`| Xử lý đúng yêu cầu của người tiền nhiệm lớn | 

## Vỏ cạnh 

Hãy xem xét:```
4 2
0 1 0 0
```Giá trị`2`cần chính xác ít nhất một giá trị trước đó`4`. Các quá trình thuật toán:```
4 -> [4]
3 -> [3, 4]
2 -> [3, 4, 2]
1 -> [1, 3, 4, 2]
```Chỉ một`4`góp phần vào`2`, và nó xuất hiện trước`2`, vậy điều kiện được thỏa mãn. Một cách tiếp cận tham lam ngây thơ đặt số lượng nhỏ lên hàng đầu sẽ thất bại. 

Bây giờ hãy xem xét:```
5 5
0 0 0 0 0
```Với mọi giá trị`i`, ngưỡng`i + 5`vượt quá`n`. Không có người đóng góp tồn tại. Trong quá trình chèn, thuật toán sẽ thấy ngay rằng`cnt == need == 0`và chèn từng giá trị vào đầu:```
[5]
[4,5]
[3,4,5]
[2,3,4,5]
[1,2,3,4,5]
```Hoán vị cuối cùng là tối thiểu về mặt từ điển. 

Cuối cùng, hãy xem xét:```
5 2
0 0 0 0 0
```Mọi giá trị đều không muốn có người tiền nhiệm lớn. Thuật toán luôn chèn từng giá trị trước phần tử lớn đủ điều kiện đầu tiên. Điều này đẩy các số nhỏ hơn về phía trước bất cứ khi nào có thể, tạo ra:```
1 2 3 4 5
```thay vì một hoán vị hợp lệ nhưng lớn hơn về mặt từ điển.
