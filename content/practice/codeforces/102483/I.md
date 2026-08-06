---
title: "CF 102483I - Lạm phát"
description: "Có những quả bóng bay có dung tích 1, 2, ..., n và những bình khí chứa một lượng nguyên khí heli. Mỗi hộp phải được gán cho chính xác một quả bóng và lượng heli trong hộp không thể chia nhỏ. Một quả bóng không thể nhận được nhiều khí heli hơn sức chứa của nó."
date: "2026-08-05T18:49:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102483
codeforces_index: "I"
codeforces_contest_name: "2018-2019 ICPC Northwestern European Regional Programming Contest (NWERC 2018)"
rating: 0
weight: 102483
solve_time_s: 199
verified: true
draft: false
---

[CF 102483I - Lạm phát](https://codeforces.com/problemset/problem/102483/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có bóng bay với công suất`1, 2, ..., n`và các bình khí chứa một lượng lớn khí heli. Mỗi hộp phải được gán cho chính xác một quả bóng và lượng heli trong hộp không thể chia nhỏ. Một quả bóng không thể nhận được nhiều khí heli hơn sức chứa của nó. 

Mục tiêu không phải là tối đa hóa tổng lượng khí heli. Thay vào đó, sau khi chọn bài tập, chúng ta xem quả bóng được lấp đầy đến phần nhỏ nhất có thể chứa được. Chúng tôi muốn làm cho quả bóng bay tồi tệ nhất đó đầy nhất có thể. 

Đầu vào cung cấp số lượng bóng bay và lượng khí heli trong tất cả các hộp. Đầu ra là giá trị lớn nhất có thể`f`sao cho mỗi quả bóng có thể kết thúc với ít nhất`f`công suất của nó đã được lấp đầy. Nếu ngay cả một nhiệm vụ hợp lệ để tránh cháy nổ không tồn tại thì câu trả lời là`-1`. 

Ràng buộc`n <= 2 * 10^5`loại trừ bất cứ điều gì thử nhiều bài tập. có`n!`có thể ghép nối giữa bóng bay và hộp, do đó không thể tìm kiếm trực tiếp ngay cả đối với đầu vào rất nhỏ. Cần có một giải pháp hợp lệ`O(n log n)`hoặc tương tự, vì việc sắp xếp vài trăm nghìn giá trị có thể chấp nhận được nhưng phép tính bậc hai thì không. 

Trường hợp cạnh đầu tiên là một hộp đựng quá lớn để có thể ghép được mọi thứ. Ví dụ:```
1
2
```Quả bóng duy nhất có sức chứa`1`, nhưng hộp chứa`2`đơn vị. Đầu ra đúng là:```
-1
```Một giải pháp bất cẩn chỉ tối ưu hóa phần tối thiểu có thể tạo ra giá trị dương vì hộp đã "đầy", nhưng nó bỏ qua hạn chế nổ. 

Một trường hợp phức tạp khác là hộp rỗng:```
3
0 2 3
```Sau khi phân loại các hộp trở thành`0, 2, 3`. Quả bóng đầu tiên lấy được hộp rỗng nên phân số nhỏ nhất là`0`. Đầu ra đúng là:```
0.0
```Một giải pháp giả định rằng mọi quả bóng đều có thể nhận được một số tiền dương nào đó sẽ thất bại ở đây. 

Trường hợp thứ ba là nên ghép hộp lớn với bóng bay lớn:```
3
1 1 3
```Bài tập được sắp xếp sẽ cho phân số`1/1`,`1/2`, Và`3/3`, vậy câu trả lời là`0.5`. Một cách tiếp cận tham lam đặt hộp lớn nhất lên quả bóng nhỏ nhất sẽ ngay lập tức đưa ra một phép gán không hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi hoán vị có thể có của các hộp. Đối với mỗi bài tập, nó sẽ kiểm tra xem có quả bóng nào phát nổ hay không và tính toán phần lấp đầy tối thiểu. Cách tiếp cận này đúng vì nó xem xét mọi sự sắp xếp có thể, nhưng số lượng nhiệm vụ`n!`, gần như không thể sử dụng được ngay lập tức. 

Một phiên bản ít cực đoan hơn sẽ tìm kiếm câu trả lời nhị phân và chạy thuật toán phù hợp cho từng phân số có thể. Đối với một phân số cố định`x`, mỗi quả bóng có kích thước`s`cần một hộp chứa giữa`x*s`Và`s`khí heli. Một thuật toán khớp chung sẽ hoạt động nhưng không cần thiết vì các quả bóng có cấu trúc rất đặc biệt. 

Quan sát quan trọng là sức chứa của khinh khí cầu đã được sắp xếp:`1, 2, ..., n`. Nếu các hộp được sắp xếp ngày càng nhiều như`c1 <= c2 <= ... <= cn`, nhiệm vụ tốt nhất có thể là ghép hộp nhỏ nhất với quả bóng nhỏ nhất, hộp nhỏ thứ hai với quả bóng nhỏ thứ hai, v.v. 

Lý do là việc gán một hộp lớn hơn cho một quả bóng nhỏ hơn chỉ có thể làm cho việc hạn chế dung lượng trở nên khó khăn hơn, trong khi việc di chuyển các hộp nhỏ hơn về phía các quả bóng nhỏ hơn sẽ cải thiện cơ hội đáp ứng tất cả các giới hạn dưới và trên. Sau khi sắp xếp, chúng ta chỉ cần kiểm tra độc lập từng vị trí. 

Đối với bóng bay`i`, hộp được chỉ định phải thỏa mãn:```
x * i <= ci <= i
```Giới hạn trên kiểm tra xem quả bóng có nổ hay không. Giới hạn dưới xác định phần nhỏ nhất có thể. Câu trả lời tốt nhất đơn giản là giá trị nhỏ nhất của`ci / i`sau khi sắp xếp, miễn là tất cả các giới hạn trên đều hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả số lượng hộp theo thứ tự không giảm. Thứ tự được sắp xếp đại diện cho nhiệm vụ duy nhất mà chúng ta cần xem xét, bởi vì hộp nhỏ nhất sẽ thuộc về quả bóng nhỏ nhất. 
2. Kiểm tra hộp đã sắp xếp tại vị trí`i`chống lại khinh khí cầu`i + 1`. Nếu hộp chứa nhiều hơn`i + 1`helium, quả bóng đó sẽ nổ tung và không có sự phân công hợp lệ nào tồn tại. 
3. Với mỗi quả bóng đúng, hãy tính phân số`canister / capacity`. Câu trả lời là giá trị nhỏ nhất của các phân số này vì mục tiêu là làm tối đa quả bóng được lấp đầy ít nhất. 
4. In phân số tối thiểu với độ chính xác vừa đủ. 

Tại sao nó hoạt động: sau khi sắp xếp, mỗi tiền tố của hộp chứa số lượng nhỏ nhất hiện có. Nếu nhỏ nhất`k`hộp có thể lấp đầy nhỏ nhất`k`bong bóng theo thứ tự được sắp xếp, bất kỳ nhiệm vụ nào khác đều không thể cải thiện quả bóng yếu nhất vì việc di chuyển hộp lớn hơn sang quả bóng nhỏ hơn chỉ tiêu tốn tài nguyên tốt hơn ở nơi nó ít hữu ích hơn. Do đó, việc ghép nối được sắp xếp sẽ mang lại phần tối thiểu tối đa có thể. Tỷ lệ tối thiểu trong cặp này chính xác là bong bóng giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)
    c = list(map(int, input().split()))
    c.sort()

    ans = 1.0
    for i, x in enumerate(c, start=1):
        if x > i:
            print(-1)
            return
        ans = min(ans, x / i)

    print("{:.10f}".format(ans))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ sắp xếp các hộp để có thể thực hiện ghép nối tham lam. Vòng lặp sử dụng`start=1`bởi vì sức chứa của khinh khí cầu là`1`bởi vì`n`, không`0`bởi vì`n-1`. 

điều kiện`x > i`kiểm tra quy tắc nổ. Điều này phải xảy ra trước khi tính toán câu trả lời vì quả bóng bay tràn ra khiến toàn bộ bài tập không thể thực hiện được. 

Biến`ans`lưu trữ phần bong bóng yếu nhất được thấy cho đến nay. Độ chính xác của dấu phẩy động Python ở đây là đủ vì khả năng chịu lỗi yêu cầu là`1e-6`. 

Không có vấn đề tràn số nguyên trong Python. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, việc so sánh vẫn phải được thực hiện cẩn thận vì giá trị ban đầu có thể lớn bằng`200000`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, các hộp được sắp xếp và ghép với bóng bay: 

| Công suất bóng | Hộp đựng | Phân số | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | 1 | 1.0 | 1.0 | 
| 2 | 2 | 1.0 | 1.0 | 
| 3 | 2 | 0,666... ​​| 0,666... ​​| 
| 4 | 3 | 0,75 | 0,666... ​​| 
| 5 | 3 | 0,6 | 0,6 | 

Phân số nhỏ nhất là`0.6`, vì vậy không có sự chuyển nhượng nào có thể đảm bảo tỷ lệ lấp đầy tối thiểu tốt hơn. 

Đối với mẫu thứ hai:```
2
2 2
```Các hộp đựng đã được sắp xếp sẵn rồi`[2, 2]`. 

| Công suất bóng | Hộp đựng | Có hiệu lực? | 
| --- | --- | --- | 
| 1 | 2 | Không | 

Quả bóng đầu tiên sẽ nhận được gấp đôi sức chứa của nó. Thuật toán xuất ra ngay lập tức`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính đơn | 
| Không gian | O(n) | Mảng số lượng hộp được lưu trữ | 

Đầu vào lớn nhất có`200000`hộp, do đó việc sắp xếp theo một lượt dễ dàng phù hợp với giới hạn thời gian và giới hạn bộ nhớ. 

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
    return result.strip()

# minimum size
assert run("1\n0\n") == "0.0000000000"

# provided sample 1
assert run("5\n1 3 2 2 3\n") == "0.6000000000"

# impossible overflow
assert run("2\n2 2\n") == "-1"

# all equal values
assert run("4\n1 1 1 1\n") == "0.2500000000"

# catches ordering mistakes
assert run("3\n1 1 3\n") == "0.5000000000"

# maximum-size style input
assert run("5\n1 2 3 4 5\n") == "1.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`0.0`| Bóng đơn và hộp rỗng | 
|`1 3 2 2 3`|`0.6`| Ghép đôi tham lam bình thường | 
|`2 2`|`-1`| Phát hiện vụ nổ | 
|`1 1 1 1`|`0.25`| Tỷ lệ nhỏ và giá trị lặp lại | 
|`1 1 3`|`0.5`| Bài tập được sắp xếp đúng | 
|`1 2 3 4 5`|`1.0`| Hộp đựng hoàn hảo | 

## Vỏ cạnh 

Đối với trường hợp hộp đựng tràn:```
1
2
```Danh sách được sắp xếp chứa một hộp có giá trị`2`. Thuật toán so sánh nó với sức chứa của quả bóng bay`1`và phát hiện`2 > 1`, vì vậy nó trả về`-1`trước khi tính bất kỳ phân số nào. 

Đối với hộp rỗng:```
3
0 2 3
```Thứ tự sắp xếp không thay đổi. Các phân số là`0/1`,`2/2`, Và`3/3`. Tối thiểu là`0`, đó là câu trả lời tốt nhất có thể vì một quả bóng phải trống. 

Đối với các trường hợp thứ tự quan trọng:```
3
1 1 3
```Bài tập được sắp xếp sẽ cung cấp dung lượng bong bóng`1, 2, 3`với khí heli`1, 1, 3`. Các phân số là`1`,`0.5`, Và`1`. Thuật toán trả về`0.5`. Việc gán hộp lớn nhất cho quả bóng nhỏ nhất trước tiên sẽ không cải thiện phân số tối thiểu và có thể tạo ra trạng thái không hợp lệ trong các đầu vào phức tạp hơn.
