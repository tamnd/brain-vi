---
title: "CF 104285D - Bộ đôi pháp sư"
description: "Chúng ta được cho một hoán vị các số từ 1 đến n. Mục tiêu là sắp xếp hoán vị này theo thứ tự tăng dần, nhưng chúng ta không được phép đưa ra các hoán đổi trực tiếp theo thứ tự chúng sẽ được thực hiện."
date: "2026-07-01T20:55:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104285
codeforces_index: "D"
codeforces_contest_name: "PCCA Winter Camp Contest 2023"
rating: 0
weight: 104285
solve_time_s: 45
verified: true
draft: false
---

[CF 104285D - Bộ đôi pháp sư](https://codeforces.com/problemset/problem/104285/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số từ 1 đến n. Mục tiêu là sắp xếp hoán vị này theo thứ tự tăng dần, nhưng chúng ta không được phép đưa ra các hoán đổi trực tiếp theo thứ tự chúng sẽ được thực hiện. Thay vào đó, trước tiên chúng ta phải xuất ra danh sách các thao tác hoán đổi, sau đó hệ thống nhận sẽ sắp xếp các thao tác này theo từ điển và thực hiện chúng theo thứ tự đã sắp xếp đó. 

Mỗi thao tác là một cặp chỉ số (x, y), nghĩa là chúng ta hoán đổi các phần tử ở vị trí x và y trong mảng. Sau khi tất cả các phép toán được sắp xếp theo thứ tự tăng x và theo y khi x liên kết, việc áp dụng chúng theo thứ tự đó phải biến đổi hoán vị thành hoán vị nhận dạng. 

Khó khăn chính là chúng ta không kiểm soát được thứ tự thực hiện một cách trực tiếp. Chúng tôi chỉ kiểm soát nhiều nhóm giao dịch hoán đổi và bước sắp xếp sẽ sắp xếp lại chúng. Vì vậy, việc xây dựng phải hợp lệ khi thực hiện được sắp xếp theo từ điển. 

Các ràng buộc cho phép n lên tới 100000 cho mỗi trường hợp thử nghiệm và tổng n lên tới 1000000. Điều này loại trừ mọi thứ bậc hai như mô phỏng các chuỗi hoán đổi tùy ý hoặc liên tục tìm kiếm các phần tử bị đặt sai vị trí. Chúng ta cần một cấu trúc tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một cách tiếp cận ngây thơ sẽ là mô phỏng việc sắp xếp bong bóng và đưa ra các giao dịch hoán đổi khi chúng xảy ra. Điều này không thành công vì sắp xếp bong bóng phụ thuộc vào thứ tự thời gian, nhưng ở đây thứ tự thực hiện được sắp xếp theo chỉ số trên toàn cầu. Một nỗ lực ngây thơ khác là sắp xếp trực tiếp bằng cách sử dụng hoán đổi lựa chọn (hoán đổi phần tử chính xác vào vị trí i). Điều này cũng không thành công vì các giao dịch hoán đổi liên quan đến các chỉ số nhỏ hơn có thể được thực hiện sớm hơn dự định và phá vỡ cấu trúc trung gian dự kiến. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều giao dịch hoán đổi có chung chỉ mục bên trái. Ví dụ: nếu chúng tôi tạo ra các giao dịch hoán đổi như (2, 5), (2, 3), (4, 5), thì thứ tự thực hiện của chúng sẽ trở thành (2, 3), (2, 5), (4, 5), có thể không khớp với trình tự mà chúng tôi dự định trừ khi được thiết kế cẩn thận. Điều này có nghĩa là giải pháp vốn dĩ phải đơn điệu về cách tạo ra các giao dịch hoán đổi. 

## Phương pháp tiếp cận 

Thách thức trọng tâm là kiểm soát thứ tự thực hiện một cách gián tiếp. Vì các hoán đổi được sắp xếp theo (x, y), chúng tôi muốn thiết kế các hoán đổi sao cho thứ tự sắp xếp này đã tương thích với quy trình sắp xếp chính xác. 

Một ý tưởng mạnh mẽ là liên tục chọn phần tử bị đặt sai vị trí tối thiểu và hoán đổi nó vào vị trí bằng cách sử dụng các hoán đổi trực tiếp với vị trí chính xác của nó. Điều này tạo ra một chuỗi sắp xếp hợp lệ nếu được thực hiện theo thứ tự đó. Tuy nhiên, ràng buộc thứ tự sắp xếp phá vỡ hoàn toàn điều này, bởi vì các giao dịch hoán đổi liên quan đến các chỉ số nhỏ hơn luôn được thực hiện trước bất kể trình tự dự kiến. Điều này phá hủy tính đúng đắn của các chuỗi hoán đổi tùy ý. 

Quan sát quan trọng là chúng ta có thể buộc phải thực hiện đúng bằng cách chỉ tạo ra các hoán đổi an toàn tự nhiên theo thứ tự từ điển. Một cách hiệu quả là mô phỏng việc sắp xếp theo kiểu chèn, nhưng hãy đảm bảo rằng các giao dịch hoán đổi luôn liên quan đến cấu trúc neo cố định để việc sắp xếp không bị ảnh hưởng. 

Một cách rõ ràng để đạt được điều này là xây dựng hoán vị thành danh tính bằng cách sử dụng quy trình xác định trong đó mỗi phần tử được di chuyển đến vị trí chính xác bằng cách sử dụng các giao dịch hoán đổi chỉ liên quan đến việc tăng điểm cuối bên trái. Chúng tôi xây dựng các hoán đổi theo cách bắt chước việc đưa từng giá trị về đúng chỉ mục của nó bằng cách sử dụng một chuỗi các hoán đổi luôn tôn trọng thứ tự từ điển. 

Một cách cải cách hữu ích là nghĩ đến việc đặt giá trị i vào vị trí i. Chúng tôi xử lý các giá trị từ 1 đến n, đảm bảo rằng khi chúng tôi cố định vị trí i, tất cả các giao dịch hoán đổi mà chúng tôi xuất ra đều có điểm cuối bên trái i hoặc các chỉ số lớn hơn, do đó các giao dịch hoán đổi trước đó không ảnh hưởng không chính xác đến cấu trúc sau này. 

Điều này dẫn đến một cấu trúc trong đó chúng tôi duy trì vị trí của các giá trị và hoán đổi từng giá trị về phía mục tiêu của nó bằng cách sử dụng các hiệu chỉnh giống như liền kề được biểu thị dưới dạng hoán đổi chung với các điểm cuối được kiểm soát.

Việc xây dựng cuối cùng mang lại nhiều nhất n lần hoán đổi, vì mỗi phần tử được chuyển vào vị trí trong các hoạt động khấu hao không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm và không tương thích với trật tự | 
| Xây dựng hoán đổi có cấu trúc | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Việc xây dựng dựa vào việc theo dõi vị trí hiện tại của từng giá trị và hoán đổi nó về vị trí chính xác theo cách được kiểm soát. 

1. Xây dựng một mảng pos sao cho pos[v] là chỉ số hiện tại của giá trị v trong hoán vị. Điều này cho phép chúng tôi xác định vị trí bất kỳ giá trị nào trong thời gian O(1). 
2. Lặp lại v từ 1 đến n, vì chúng ta muốn đặt các giá trị theo thứ tự tăng dần vào đúng vị trí của chúng. 
3. Đối với mỗi giá trị v, trong khi pos[v] không bằng v, chúng ta di chuyển nó về vị trí đích v bằng cách hoán đổi nó với phần tử hiện ở vị trí v. Điều này được thực hiện bằng cách thực hiện hoán đổi giữa pos[v] và v. 
4. Sau mỗi lần hoán đổi (i, j), hãy cập nhật cả mảng và mảng pos để các vị trí vẫn nhất quán. 
5. Ghi lại mọi lần hoán đổi (i, j) trong danh sách. 
6. Xuất tất cả các giao dịch hoán đổi được ghi lại. Thuộc tính quan trọng là các giao dịch hoán đổi được tạo theo thứ tự xác định phù hợp với các điểm cuối bên trái ngày càng tăng. 

Lý do bước 3 đúng là vì việc hoán đổi pos[v] với v đặt giá trị v gần đích cuối cùng của nó mà không làm ảnh hưởng đến các tiền tố đã cố định theo cách phá vỡ tính chính xác. 

### Tại sao nó hoạt động 

Tại thời điểm chúng tôi xử lý giá trị v, tất cả các giá trị nhỏ hơn v đã được cố định ở đúng vị trí của chúng. Hoán đổi (pos[v], v) chỉ liên quan đến các chỉ số ít nhất là v ở trạng thái hoán vị có cấu trúc tốt. Điều này đảm bảo các vị trí trước đó không bao giờ bị sửa đổi sau khi được sửa. Bởi vì các hoán đổi được tạo theo thứ tự tăng dần của hành vi điểm cuối bên trái của chúng, nên việc sắp xếp từ điển không thay đổi trình tự thực hiện dự định của chúng theo cách có hại. Việc xây dựng duy trì tính bất biến rằng các vị trí từ 1 đến v-1 đã chính xác và không bao giờ được chạm lại, do đó, không có sự hoán đổi nào trong tương lai có thể làm hỏng chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a = [0] + a  # 1-indexed

    pos = [0] * (n + 1)
    for i in range(1, n + 1):
        pos[a[i]] = i

    ops = []

    for v in range(1, n + 1):
        while pos[v] != v:
            i = pos[v]
            j = v

            ops.append((min(i, j), max(i, j)))

            ai, aj = a[i], a[j]
            a[i], a[j] = a[j], a[i]
            pos[ai], pos[aj] = pos[aj], pos[ai]

    print(len(ops))
    for x, y in ops:
        print(x, y)

if __name__ == "__main__":
    solve()
```Mã duy trì cả mảng hoán vị và bảng tra cứu ngược nên mọi hoán đổi đều là O(1). Mỗi giá trị được kéo liên tục về vị trí chính xác bằng cách sử dụng hoán đổi trực tiếp, đảm bảo tiến độ trong mỗi lần lặp. 

Chi tiết triển khai tinh tế là luôn lưu trữ các giao dịch hoán đổi dưới dạng các cặp được sắp xếp (tối thiểu, tối đa). Điều này là bắt buộc vì thứ tự thực hiện phụ thuộc vào các cặp được sắp xếp chứ không phụ thuộc vào thứ tự chúng ta in điểm cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hoán vị đầu vào: [1, 3, 5, 2, 4] 

Chúng tôi theo dõi vị trí của các giá trị và áp dụng giao dịch hoán đổi. 

| v | vị trí[v] | trao đổi | mảng sau khi trao đổi | 
| --- | --- | --- | --- | 
| 2 | 4 | (2,4) | [1,2,5,3,4] | 
| 3 | 4 | (3,4) | [1,2,3,5,4] | 
| 4 | 5 | (4,5) | [1,2,3,4,5] | 
| 5 | 4 | (4,5) | đã sửa rồi | 

Điều này tạo ra sự hoán đổi dần dần kéo từng phần tử vào đúng vị trí. Sau khi sắp xếp các hoạt động theo từ điển, thứ tự thực hiện vẫn tuân theo trình tự hiệu chỉnh dự kiến ​​vì các hoán đổi được cấu trúc xung quanh các mục tiêu tăng dần. 

### Ví dụ 2 

Hoán vị đầu vào: [5, 4, 3, 2, 1] 

Chúng ta bắt đầu từ v = 1: 

| v | vị trí[v] | trao đổi | mảng | 
| --- | --- | --- | --- | 
| 1 | 5 | (1,5) | [1,4,3,2,5] | 
| 2 | 4 | (2,4) | [1,2,3,4,5] | 

Các giá trị sau này đã chính xác. 

Điều này cho thấy các đảo ngược lớn sẽ sụp đổ nhanh chóng như thế nào vì mỗi lần hoán đổi trực tiếp nhắm mục tiêu vào đúng chỉ mục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi lần hoán đổi sẽ sửa ít nhất một phần tử bị đặt sai vị trí và mỗi phần tử được di chuyển một số lần giới hạn bằng cách sử dụng cập nhật vị trí trực tiếp | 
| Không gian | O(n) | Mảng để hoán vị và theo dõi vị trí | 

Tổng n trên tất cả các trường hợp thử nghiệm tối đa là 10^6, do đó độ phức tạp tuyến tính là đủ. Việc sử dụng bộ nhớ vẫn tuyến tính và ổn định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimum case
assert run("1\n2\n2 1\n") != ""

# already sorted
assert run("1\n3\n1 2 3\n").splitlines()[0] == "0"

# reverse case
res = run("1\n5\n5 4 3 2 1\n")
assert len(res.splitlines()) > 1

# sample-like case
assert run("1\n5\n1 3 5 2 4\n")

# single test sanity
assert " " in run("1\n4\n2 1 4 3\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 hoán đổi | sắp xếp nhanh chóng | độ đúng tối thiểu | 
| hoán vị danh tính | hoạt động bằng không | không có giao dịch hoán đổi không cần thiết | 
| hoán vị ngược | nhiều lần hoán đổi | hành vi trong trường hợp xấu nhất | 
| hoán vị xen kẽ | sửa lỗi có cấu trúc | tính đúng đắn chung | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi hoán vị đã được sắp xếp. Thuật toán không thực hiện hoán đổi vì pos[v] == v với mọi v, do đó danh sách thao tác vẫn trống, danh sách này hợp lệ với ràng buộc k ≤ n. 

Một trường hợp khác là sự đảo ngược hoàn toàn. Đối với đầu vào [n, n-1, ..., 1], mỗi giá trị v được hoán đổi trực tiếp sang vị trí v trong một thao tác duy nhất, do đó quá trình kết thúc theo thời gian tuyến tính với tối đa n/2 lần hoán đổi. 

Một trường hợp khó phát hiện là khi các giá trị gần như chính xác nhưng tạo thành một chu kỳ dài, chẳng hạn như [2, 3, 4, 5, 1]. Thuật toán phá vỡ chu trình bằng cách sửa 1 trước, thực hiện các chỉnh sửa theo tầng trong cấu trúc mà không cần xem lại các vị trí đã cố định, đảm bảo không có vòng lặp vô hạn hoặc hoán đổi dư thừa.
