---
title: "CF 1326A - Những con số xấu xấu"
description: "Chúng ta cần xây dựng một số thập phân dương có đúng n chữ số. Mọi chữ số phải khác 0 và toàn bộ số phải không chia hết cho mỗi chữ số xuất hiện trong đó. Đối với mỗi trường hợp thử nghiệm, đầu vào chỉ cung cấp độ dài n yêu cầu."
date: "2026-06-11T16:34:05+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1326
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 7"
rating: 1000
weight: 1326
solve_time_s: 175
verified: false
draft: false
---

[CF 1326A - Những con số xấu xấu](https://codeforces.com/problemset/problem/1326/A) 

**Đánh giá:** 1000 
**Tas:** giải thuật xây dựng, lý thuyết số 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một số thập phân dương với chính xác`n`chữ số. Mọi chữ số phải khác 0 và toàn bộ số phải không chia hết cho mỗi chữ số xuất hiện trong đó. 

Đối với mỗi trường hợp thử nghiệm, đầu vào chỉ cung cấp độ dài cần thiết`n`. Chúng tôi có thể xuất ra bất kỳ số hợp lệ nào có độ dài đó. Nếu không có số đó tồn tại, chúng tôi in`-1`. 

Các ràng buộc ngay lập tức gợi ý rằng chúng ta không cần phải tìm kiếm thông qua số lượng ứng cử viên. Một trường hợp thử nghiệm đơn lẻ có thể yêu cầu tạo ra một số có tối đa`10^5`các chữ số và tổng của tất cả các độ dài cũng lên tới`10^5`. Bất kỳ thuật toán nào kiểm tra nhiều ứng viên hoặc thực hiện các phép tính tốn kém trên các số nguyên lớn đều không cần thiết. Giải pháp dự định phải xây dựng câu trả lời trực tiếp theo thời gian tỷ lệ thuận với kích thước đầu ra. 

Trường hợp cạnh không rõ ràng đầu tiên là`n = 1`. 

đầu vào:```
1
1
```Đầu ra:```
-1
```Số dương có một chữ số luôn chia hết cho chính nó. Vì chữ số duy nhất trong số bằng số đó nên không tồn tại câu trả lời hợp lệ. 

Một lỗi dễ mắc khác là xây dựng một số bao gồm toàn bộ`9`S. 

đầu vào:```
1
3
```Ứng viên:```
999
```Điều này thất bại vì`999`chia hết cho`9`. Không có chữ số 0 là không đủ. 

Sai lầm thứ ba là quên rằng tính chia hết phải không có đối với mọi chữ số có mặt. Ví dụ:```
444
```chia hết cho`4`, vì vậy nó không hợp lệ mặc dù tất cả các chữ số đều khác 0. 

Thách thức là không kiểm tra khả năng chia hết một cách hiệu quả. Thách thức là tìm ra một mẫu được đảm bảo hoạt động với mọi độ dài lớn hơn một. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ tạo ra`n`-chữ số và kiểm tra xem mỗi ứng cử viên có thỏa mãn điều kiện hay không. Việc kiểm tra một ứng viên rất dễ dàng: kiểm tra các chữ số của nó và kiểm tra khả năng chia hết cho từng chữ số. Vấn đề là không gian tìm kiếm. có`9^n`số với`n`các chữ số khác 0. Ngay cả đối với`n = 20`, cái này đã lớn về mặt thiên văn rồi. Vì`n = 100000`, việc tìm kiếm toàn diện là hoàn toàn không thể. 

Quan sát quan trọng là chúng ta chỉ cần một câu trả lời hợp lệ chứ không phải tất cả. 

Giả sử chúng ta xây dựng một số chỉ chứa các chữ số`2`Và`3`. Nếu số đó là số lẻ thì không chia hết cho`2`. Nếu tổng các chữ số của nó không phải là bội số của`3`, thì nó không thể chia hết cho`3`. Vì đó là những chữ số duy nhất xuất hiện trong số nên cả hai yêu cầu đều được đáp ứng. 

Một mẫu rất đơn giản đạt được chính xác điều này:```
2333...333
```Chữ số đầu tiên là`2`, và phần còn lại`n - 1`chữ số là`3`. 

Hãy để chúng tôi xác minh nó. 

Chữ số cuối cùng là`3`nên số đó là số lẻ và không chia hết cho`2`. 

Tổng các chữ số bằng$$2 + 3(n-1)=3n-1.$$Từ`3n - 1`lá còn lại`2`khi chia cho`3`, số đó không chia hết cho`3`. 

Các chữ số duy nhất hiện diện là`2`Và`3`, do đó số này không chia hết cho bất kỳ chữ số nào của nó. 

Công trình này hoạt động cho mọi`n > 1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(9^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`. 
2. Nếu`n = 1`, in`-1`. 

Số dương có một chữ số luôn chia hết cho chữ số duy nhất của nó nên không tồn tại nghiệm. 
3. Ngược lại, xuất ra chuỗi gồm một`'2'`theo sau là`n - 1`bản sao của`'3'`. 

Điều này mang lại một`n`-số có chữ số chỉ chứa các chữ số`2`Và`3`. 

### Tại sao nó hoạt động 

Đối với mọi`n > 1`, số được xây dựng kết thúc bằng chữ số`3`, thế thì thật kỳ lạ. Bất kỳ số nào chia hết cho`2`phải là số chẵn nên số đó không chia hết cho`2`. 

Tổng các chữ số là$$2 + 3(n-1)=3n-1.$$Giá trị này không chia hết cho`3`, nên bản thân số đó không chia hết cho`3`. 

Các chữ số duy nhất xuất hiện trong số đó là`2`Và`3`. Chúng tôi đã chỉ ra rằng số này không chia hết cho cả hai. Do đó, nó không chia hết cho bất kỳ chữ số nào xuất hiện trong biểu diễn thập phân của nó. 

Vì`n = 1`, không tồn tại số hợp lệ nên việc in`-1`là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        if n == 1:
            ans.append("-1")
        else:
            ans.append("2" + "3" * (n - 1))

    sys.stdout.write("\n".join(ans))

solve()
```Chương trình xử lý từng trường hợp thử nghiệm một cách độc lập. 

Trường hợp đặc biệt`n == 1`phải được xử lý trước tiên. Quên trường hợp này sẽ tạo ra`"2"`, điều này không hợp lệ vì`2`chia hết cho`2`. 

Đối với tất cả các độ dài lớn hơn, biểu thức`"2" + "3" * (n - 1)`trực tiếp xây dựng chuỗi yêu cầu. Làm việc với chuỗi được ưu tiên hơn là xây dựng một số nguyên khổng lồ vì`n`có thể đạt được`100000`. Bài toán chỉ yêu cầu chúng ta in số ra chứ không thực hiện phép tính trên đó. 

Đầu ra được tích lũy thành một danh sách và được in một lần ở cuối. Điều này tránh các hoạt động đầu ra lặp đi lặp lại và phù hợp với thực tiễn lập trình cạnh tranh tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 1
```| Bước | n | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | Kiểm tra trường hợp đặc biệt |`-1`| 

Ví dụ này cho thấy tình huống không thể duy nhất. Bất kỳ số dương có một chữ số nào cũng bằng chữ số duy nhất của nó và chia hết cho nó. 

### Ví dụ 2 

đầu vào:```
n = 4
```| Bước | n | Hành động | Kết quả hiện tại | 
| --- | --- | --- | --- | 
| 1 | 4 |`n != 1`| Tiếp tục | 
| 2 | 4 | Xây dựng`"2" + "3" * 3`|`2333`| 
| 3 | 4 | In câu trả lời |`2333`| 

Xác minh: 

| Tài sản | Giá trị | 
| --- | --- | 
| Chữ số hiện tại | 2, 3 | 
| Chữ số cuối cùng | 3 | 
| Chia hết cho 2? | Không | 
| Tổng chữ số | 11 | 
| Chia hết cho 3? | Không | 

Dấu vết này xác nhận sự bất biến đằng sau việc xây dựng. Số đó không chia hết cho`2`cũng không phải bởi`3`, bao gồm mọi chữ số xuất hiện trong đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Xây dựng chuỗi đầu ra yêu cầu viết`n`nhân vật | 
| Không gian | O(n) | Bản thân câu trả lời được tạo ra có chứa`n`nhân vật | 

Tổng chiều dài của tất cả các số được in tối đa là`10^5`, do đó công việc tuyến tính ở kích thước đầu ra dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ cũng tỷ lệ thuận với đầu ra được tạo ra và phù hợp thoải mái trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)

    def input():
        return input_data.readline()

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())

        if n == 1:
            ans.append("-1")
        else:
            ans.append("2" + "3" * (n - 1))

    return "\n".join(ans)

# provided sample
assert run("4\n1\n2\n3\n4\n") == "-1\n23\n233\n2333"

# custom cases
assert run("1\n1\n") == "-1", "minimum length"

assert run("1\n2\n") == "23", "smallest valid length"

assert run("1\n5\n") == "23333", "general construction"

assert run("1\n10\n") == "2" + "3" * 9, "larger construction"

assert len(run("1\n100000\n")) == 100000, "maximum length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n`|`-1`| Trường hợp một chữ số không thể | 
|`1\n2\n`|`23`| Công trình hợp lệ nhỏ nhất | 
|`1\n5\n`|`23333`| Tính đúng đắn chung | 
|`1\n10\n`|`2333333333`| Tạo đầu ra dài hơn | 
|`1\n100000\n`| Chiều dài`100000`chuỗi | Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
1
```Thuật toán ngay lập tức đi vào nhánh đặc biệt và in`-1`. Điều này đúng vì mọi số nguyên dương một chữ số đều chia hết cho chính nó. Không có công trình xây dựng nào có thể đáp ứng được yêu cầu. 

Hãy xem xét đầu vào:```
1
2
```Đầu ra của thuật toán`23`. Số đó là số lẻ nên không chia hết cho`2`. Tổng chữ số của nó là`5`, nên nó không chia hết cho`3`. Vì các chữ số duy nhất có mặt là`2`Và`3`, câu trả lời là hợp lệ. 

Hãy xem xét đầu vào:```
1
100000
```Thuật toán xây dựng một`'2'`theo sau là`99999`bản sao của`'3'`. Không có phép tính nào trên các số nguyên lớn được thực hiện. Đầu ra có chính xác`100000`các chữ số, vẫn là số lẻ và có tổng các chữ số$$2 + 3 \cdot 99999 = 299999,$$không chia hết cho`3`. Bằng chứng tương tự được sử dụng cho các giá trị nhỏ sẽ không thay đổi đối với kích thước đầu vào tối đa.
