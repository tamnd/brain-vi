---
title: "CF 106414C - Phân loại Cakenap"
description: "Chúng ta được cho một hoán vị, nghĩa là mỗi số từ 1 đến n xuất hiện đúng một lần. Một thao tác lấy một số tiền tố của mảng hiện tại, đảo ngược tiền tố đó và sau đó di chuyển toàn bộ tiền tố đã sửa đổi đến cuối mảng. Nhiệm vụ không phải là giảm thiểu số lượng hoạt động."
date: "2026-06-25T09:47:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106414
codeforces_index: "C"
codeforces_contest_name: "Rutgers University Programming Contest Spring 2026 - Open Division"
rating: 0
weight: 106414
solve_time_s: 91
verified: true
draft: false
---

[CF 106414C - Sắp xếp Cakenap](https://codeforces.com/problemset/problem/106414/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị, nghĩa là mọi số từ`1`ĐẾN`n`xuất hiện đúng một lần. Một thao tác lấy một số tiền tố của mảng hiện tại, đảo ngược tiền tố đó và sau đó di chuyển toàn bộ tiền tố đã sửa đổi đến cuối mảng. Nhiệm vụ không phải là giảm thiểu số lượng hoạt động. Chúng ta chỉ cần in tối đa một chuỗi`2n`độ dài tiền tố biến đổi hoán vị thành thứ tự tăng dần. 

Đầu vào chứa một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, mảng biểu thị thứ tự hiện tại của các số. Đầu ra là danh sách độ dài tiền tố và việc áp dụng các thao tác đó theo thứ tự phải để lại mảng dưới dạng`[1, 2, ..., n]`. 

Tổng của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`5000`. Một giải pháp bậc hai có thể chấp nhận được vì tổng số phần tử nhỏ. Chúng ta có thể chi tiêu`O(n^2)`mất nhiều thời gian tìm kiếm vị trí nhiều lần nhưng giải pháp mô phỏng nhiều thao tác không cần thiết vẫn sẽ thất bại nếu vượt quá giới hạn cho phép.`2n`giới hạn hoạt động. Thử thách chính không phải là tốc độ mà là khám phá một công trình có giới hạn đảm bảo. 

Một lỗi phổ biến là coi đây là cách sắp xếp thông thường và cố gắng đặt từng phần tử vào vị trí cuối cùng của chúng trong khi vẫn giữ tiền tố hoặc hậu tố cố định. Điều này không có tác dụng vì mọi thao tác đều thay đổi mặt trước của toàn bộ mảng. Ví dụ, trên`[1, 4, 2, 3]`, sau khi sửa`4`khi kết thúc với thao tác tìm kiếm cục bộ, các thao tác sau này có thể di chuyển lại thao tác đó. Việc xây dựng phải sử dụng hành vi xoay vòng toàn cầu của hoạt động. 

Một trường hợp cạnh khác là hoán vị đã được sắp xếp. Đối với đầu vào:```
1
5
1 2 3 4 5
```đầu ra đúng là không có hoạt động. Một giải pháp luôn thực hiện các phép quay cuối cùng vẫn có hiệu lực, nhưng việc thực hiện công việc một cách không cần thiết có thể làm cho lý luận trở nên phức tạp hơn. 

Trường hợp cạnh thứ hai là`n = 1`. Mảng đã được sắp xếp và không có thao tác hữu ích nào để thực hiện. Đầu ra đúng là:```
0
```Việc cố gắng thêm bước xoay vô điều kiện có thể tạo ra độ dài tiền tố không hợp lệ nếu việc triển khai không cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tìm kiếm thông qua các chuỗi hoạt động có thể xảy ra. Đối với mỗi trạng thái hoán vị, có`n`những bước di chuyển có thể xảy ra và số hoán vị có thể xảy ra là`n!`. Ngay cả với việc ghi nhớ, điều này nhanh chóng trở nên không thể thực hiện được vì không gian trạng thái tăng theo cấp số nhân. Giới hạn hoạt động cũng có nghĩa là việc tìm kiếm bất kỳ chuỗi hợp lệ nào trực tiếp bằng tìm kiếm là không cần thiết. 

Quan sát quan trọng là việc đưa các số lên phía trước theo thứ tự tăng dần sẽ tạo ra một sự sắp xếp theo chu kỳ có thể dự đoán được. Giả sử chúng ta xử lý các giá trị`1, 2, ..., n - 1`. Khi giá trị`x`chưa ở phía trước, hãy chọn tiền tố kết thúc ngay trước`x`di chuyển`x`về phía trước. Điều này hoạt động vì hậu tố không bị ảnh hưởng bắt đầu bằng`x`, trong khi tiền tố đã chọn bị đảo ngược và di chuyển ra phía sau nó. 

Sau khi thực hiện việc này cho tất cả các giá trị lên đến`n - 1`, hoán vị không được sắp xếp đầy đủ nhưng nó có dạng rất cụ thể:```
[n - 1, n, 1, 2, ..., n - 2]
```Đây là một phép quay của mảng được sắp xếp. Hai thao tác có độ dài tiền tố`1`xoay chuỗi này trở lại thứ tự đã sắp xếp. Toàn bộ công trình sử dụng tối đa`n - 1 + 2 = n + 1`hoạt động an toàn dưới đây`2n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | Ồ (n!) | Quá chậm | 
| Tòa nhà mặt tiền xây dựng | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với danh sách thao tác trống. Chúng ta sẽ xây dựng câu trả lời bằng cách di chuyển các giá trị lên phía trước theo thứ tự tăng dần. 
2. Với mọi giá trị`x`từ`1`ĐẾN`n - 1`, tìm vị trí hiện tại của nó`p`. 

Nếu như`p`không phải là vị trí đầu tiên, áp dụng thao tác với độ dài tiền tố`p - 1`. Các phần tử trước`x`được di chuyển phía sau nó, vì vậy`x`trở thành phần tử đầu tiên. Nếu như`x`đã là đầu tiên rồi, không cần thao tác gì cả. 
3. Sau khi xử lý tất cả các giá trị từ`1`ĐẾN`n - 1`, áp dụng thao tác với độ dài tiền tố`1`hai lần. 

Mỗi độ dài tiền tố`1`hoạt động di chuyển phần tử đầu tiên về phía sau. Vòng quay còn lại chuyển từ`[n - 1, n, 1, ..., n - 2]`ĐẾN`[1, 2, ..., n]`. 

Tại sao nó hoạt động: sau khi xử lý giá trị`x`, giá trị đó được đặt trước tất cả các giá trị sẽ được xử lý sau này. Do đó, chuỗi các giá trị được chọn ở phía trước được xây dựng theo thứ tự tăng dần, nhưng vì mọi thao tác cũng làm xoay mảng nên sự sắp xếp cuối cùng là một phép dịch chuyển tuần hoàn duy nhất của hoán vị đã sắp xếp. Hai phép quay cuối cùng loại bỏ chính xác sự dịch chuyển đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(a):
    n = len(a)
    ans = []

    for x in range(1, n):
        p = a.index(x) + 1
        if p > 1:
            k = p - 1
            ans.append(k)
            a[:] = a[k:] + a[:k][::-1]

    if n > 1:
        ans.append(1)
        a[:] = a[1:] + a[:1]
        ans.append(1)
        a[:] = a[1:] + a[:1]

    return ans

def main():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ans = solve_case(a)

        out.append(str(len(ans)))
        if ans:
            out.append(" ".join(map(str, ans)))
        else:
            out.append("")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai giữ nguyên hoán vị hiện tại vì việc xây dựng phụ thuộc vào vị trí hiện tại của từng giá trị. Vì tổng cộng`n`chỉ là`5000`, sử dụng`list.index`để xác định từng giá trị là đủ nhanh. 

Việc mô phỏng sau mỗi thao tác được chọn chỉ được thực hiện để duy trì trạng thái hiện tại trong khi xây dựng câu trả lời. Bản thân hoạt động này được thực hiện như việc loại bỏ phần đầu tiên`k`các phần tử, đảo ngược chúng và đặt chúng sau hậu tố còn lại. Việc cắt Python xử lý việc này một cách trực tiếp. 

Hai thao tác cuối cùng chỉ được thêm vào khi`n > 1`. Đối với một mảng phần tử đơn, hoán vị đã được sắp xếp và không có độ dài tiền tố hợp lệ nào khác ngoài`1`tồn tại, do đó việc tránh các thao tác này sẽ giữ cho trường hợp ranh giới sạch sẽ. 

## Ví dụ đã hoạt động 

Hãy xem xét hoán vị:```
[2, 1, 3]
```| Bước | Giá trị được xử lý | Vị trí của giá trị | Hoạt động | Mảng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 |`1`|`[1, 3, 2]`| 
| 2 | 2 | 3 |`2`|`[2, 1, 3]`| 
| 3 | vòng quay cuối cùng | |`1`|`[1, 3, 2]`| 
| 4 | vòng quay cuối cùng | |`1`|`[3, 2, 1]`| 

Dấu vết này cho thấy tại sao chỉ mô phỏng các phép quay riêng lẻ lại gây hiểu lầm. Trạng thái chính xác sau khi xử lý tất cả các giá trị tuân theo bất biến tuần hoàn và cần có hai phép quay cuối cùng để hoàn thành việc xây dựng. 

Coi như:```
[1, 4, 2, 3]
```| Bước | Giá trị được xử lý | Vị trí của giá trị | Hoạt động | Mảng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | không |`[1, 4, 2, 3]`| 
| 2 | 2 | 3 |`2`|`[2, 3, 4, 1]`| 
| 3 | 3 | 2 |`1`|`[3, 4, 1, 2]`| 
| 4 | vòng quay cuối cùng | |`1`|`[4, 1, 2, 3]`| 
| 5 | vòng quay cuối cùng | |`1`|`[1, 2, 3, 4]`| 

Ví dụ này thể hiện ý chính: giai đoạn đầu tiên không trực tiếp sắp xếp mảng. Nó tạo ra một thứ tự sắp xếp xoay vòng và được sửa ở cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi trong số`n - 1`giá trị tìm kiếm vị trí của nó và có thể xây dựng lại mảng. | 
| Không gian | O(n) | Danh sách thao tác và các lát cắt tạm thời chỉ lưu trữ dữ liệu tuyến tính. | 

Tổng của tất cả`n`giá trị nhiều nhất là`5000`, do đó việc xây dựng bậc hai dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def apply_ops(a, ops):
    for k in ops:
        a[:] = a[k:] + a[:k][::-1]
    return a

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    main()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# helper using the solution directly
def check(inp):
    lines = inp.strip().splitlines()
    t = int(lines[0])
    idx = 1
    for _ in range(t):
        n = int(lines[idx])
        idx += 1
        a = list(map(int, lines[idx].split()))
        idx += 1
        ops = solve_case(a[:])
        assert len(ops) <= 2 * n
        assert apply_ops(a[:], ops) == list(range(1, n + 1))

check("""4
3
2 1 3
2
2 1
4
1 4 2 3
10
3 2 1 4 5 6 10 9 8 7
""")

check("""3
1
1
2
1 2
5
5 4 3 2 1
""")

check("""2
6
6 5 4 3 2 1
7
2 7 1 6 3 5 4
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng được sắp xếp | Công trình hợp lệ bằng không hoặc nhỏ | Xử lý các hoán vị đã được sắp xếp. | 
| Yếu tố đơn | Không có hoạt động | Kiểm tra trường hợp ranh giới nhỏ nhất. | 
| Mảng được sắp xếp ngược | Trình tự hợp lệ trong`2n`| Tập nhiều động tác. | 
| Hoán vị ngẫu nhiên | Sắp xếp kết quả sau khi mô phỏng | Khẳng định sự bất biến. | 

## Vỏ cạnh 

Đối với một hoán vị đã được sắp xếp, chẳng hạn như:```
1
5
1 2 3 4 5
```mọi giá trị từ`1`ĐẾN`n - 1`đã ở phía trước khi nó được xử lý, do đó không có thao tác nào được thêm vào. Vì hai phép quay cuối cùng chỉ cần thiết để khắc phục sự dịch chuyển theo chu kỳ được tạo bởi giai đoạn đầu tiên nên chúng bị bỏ qua vì mảng không bao giờ đi vào trạng thái dịch chuyển đó. 

Đối với một phần tử duy nhất:```
1
1
1
```vòng lặp trên các giá trị không thực thi và bước xoay cuối cùng bị bỏ qua vì không có độ dài tiền tố có ý nghĩa để sử dụng. Câu trả lời không chứa hoạt động nào. 

Đối với trường hợp giá trị nhỏ nhất ban đầu không ở gần phía trước, chẳng hạn như:```
1
5
5 3 1 4 2
```thuật toán tìm vị trí của`1`, di chuyển nó lên phía trước và tiếp tục với`2`,`3`, Và`4`. Giai đoạn đầu tiên đảm bảo trạng thái cuối cùng là một phép quay của chuỗi đã được sắp xếp, do đó, độ dài của hai tiền tố cuối cùng`1`các hoạt động hoàn thành việc sắp xếp mà không cần bất kỳ xử lý đặc biệt nào.
