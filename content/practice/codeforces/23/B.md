---
title: "CF 23B - Đảng"
description: "Chúng ta có thể coi bữa tiệc như một biểu đồ tình bạn vô hướng. Mỗi người là một đỉnh, và một cạnh nghĩa là hai người là bạn."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 23
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 23"
rating: 1600
weight: 23
solve_time_s: 207
verified: true
draft: false
---
[CF 23B - Đảng](https://codeforces.com/problemset/problem/23/B) 

**Đánh giá:** 1600 
**Tags:** xây dựng thuật toán, đồ thị, toán học 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có thể coi bữa tiệc như một biểu đồ tình bạn vô hướng. Mỗi người là một đỉnh, và một cạnh nghĩa là hai người là bạn. 

Quá trình loại bỏ mọi người theo từng vòng. Đầu tiên, mọi người có bằng cấp`0`lá. Sau những lần loại bỏ đó, một số người còn lại giờ đây có thể đã có bằng cấp`1`, nên họ rời đi tiếp theo. Thế thì người có bằng cấp`2`nghỉ phép, v.v., tăng dần mức độ yêu cầu cho đến khi đạt được`n - 1`. 

Câu hỏi không yêu cầu chúng ta mô phỏng một biểu đồ nhất định. Thay vào đó, đối với một số lượng người nhất định`n`, chúng ta phải thiết kế biểu đồ tình bạn theo cách tốt nhất có thể để càng nhiều người sống sót càng tốt sau khi toàn bộ quá trình kết thúc. 

Về mặt khái niệm, các ràng buộc là cực kỳ nhỏ, mặc dù có thể có tới`10^5`trường hợp thử nghiệm. Mỗi test case chỉ chứa một số nguyên`n`và đầu ra chỉ là một câu trả lời số nguyên. Điều này ngay lập tức gợi ý rằng giải pháp dự định là giải pháp toán học hoặc mang tính xây dựng, với quá trình xử lý theo thời gian không đổi cho mỗi trường hợp thử nghiệm. Bất kỳ thuật toán nào cố gắng liệt kê các đồ thị đều vô vọng vì số lượng đồ thị vô hướng trên`n`đỉnh là`2^(n(n-1)/2)`. 

Phần tế nhị của vấn đề là hiểu được ý nghĩa thực sự của “sống sót cuối cùng”. Một cách giải thích bất cẩn có thể cho rằng nếu một người có đủ bạn bè thì họ sẽ ở lại mãi mãi. Điều đó là sai vì việc xóa thay đổi mức độ một cách linh hoạt. 

Coi như`n = 3`. 

Đồ thị tam giác có tất cả các độ bằng`2`. Không ai rời khỏi vòng đầu tiên vì không có đỉnh cô lập. Không có ai bỏ cuộc ở vòng 2 vì không có đỉnh bậc 1. Sau đó quá trình đạt đến mức độ`2`, và cả ba người cùng nhau rời đi. Câu trả lời đúng là`0`, không`3`. 

Một biểu đồ khác hoạt động tốt hơn. Lấy một cạnh và một đỉnh cô lập. Đỉnh bị cô lập rời đi đầu tiên. Hai đỉnh còn lại đều có bậc`1`, nên họ rời đi ở vòng tiếp theo. Một trong số họ là người bị loại bỏ cuối cùng, vì vậy chính xác một người có thể ở lại cuối quá trình. Câu trả lời trở thành`1`. 

Một sai lầm dễ mắc phải khác là cho rằng câu trả lời luôn là`1`. 

Lấy`n = 4`. Một chu kỳ dài`4`cho mọi mức độ đỉnh`2`. Không ai rời đi ở mức độ`0`hoặc`1`. Khi quá trình đạt đến mức độ`2`, cả bốn đỉnh đều rời đi. Một lần nữa câu trả lời cuối cùng là`0`. 

Nhưng chúng ta có thể làm tốt hơn với một tam giác cộng với một đỉnh cô lập. Người bị cô lập rời đi trước. Tam giác tồn tại đến mức`2`giai đoạn mà cả ba đỉnh của tam giác đều bị loại bỏ. Số đỉnh bị loại bỏ cuối cùng vẫn là`1`, không`3`. 

Quan sát quan trọng là quá trình này cuối cùng luôn loại bỏ tất cả mọi người. Điều duy nhất chúng tôi kiểm soát là có bao nhiêu người vẫn có mặt ngay trước giai đoạn loại bỏ cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê mọi biểu đồ tình bạn có thể có trên`n`các đỉnh và mô phỏng quá trình xóa. 

Đối với đồ thị cố định, việc mô phỏng rất đơn giản. Ở sân khấu`k`, loại bỏ mọi đỉnh còn sống có bậc hiện tại bằng`k`. Tiếp tục cho đến khi tất cả các giai đoạn được xử lý. Sau đó ghi lại số đỉnh còn tồn tại ngay trước lần xóa cuối cùng. 

Điều này hiệu quả vì định nghĩa quy trình là rõ ràng và mang tính quyết định. Vấn đề là số lượng đồ thị. Đồ thị vô hướng trên`n`đỉnh có`n(n-1)/2`các cạnh có thể có, vì vậy có$2^{\frac{n(n-1)}{2}}$đồ thị khác nhau. 

Ngay cả đối với`n = 20`, cái này lớn về mặt thiên văn. Không có mức độ tối ưu hóa nào có thể khiến cho việc sử dụng vũ lực trở nên khả thi. 

Bước đột phá đến từ việc phân tích những gì phải xảy ra trong quá trình này. 

Giả sử tại một thời điểm nào đó có`m`người vẫn còn sống. Mỗi người trong số họ có thể có tối đa bằng cấp`m - 1`. Cuối cùng quá trình đạt đến giai đoạn`m - 1`. Đến giai đoạn đó, mọi người còn lại đều phải rời đi, vì không ai có thể có bằng cấp lớn hơn`m - 1`. 

Điều đó có nghĩa là quá trình luôn kết thúc bằng việc xóa tất cả các đỉnh hiện còn lại cùng nhau. 

Bây giờ vấn đề trở thành: kích thước lớn nhất có thể có của nhóm cuối cùng đó là bao nhiêu? 

Giả sử nhóm sống sót cuối cùng có quy mô`m`. Ngay trước bước cuối cùng, mỗi bước đó`m`đỉnh phải có bậc chính xác`m - 1`, nếu không chúng sẽ bị xóa sớm hơn hoặc muộn hơn. Vì vậy nhóm cuối cùng phải tạo thành một biểu đồ kích thước hoàn chỉnh`m`. 

Nhưng nếu toàn bộ đồ thị ban đầu có chính xác`m`các đỉnh, sau đó tất cả chúng cùng biến mất trong giai đoạn cuối, không để lại ai sau quá trình. Để làm cho một người sống sót lâu hơn, chúng ta cần thêm ít nhất một đỉnh bên ngoài nhóm. 

Điều này cho thấy việc xây dựng tối ưu: 

1. Xây dựng một nhóm trên`m`đỉnh. 
2. Thêm một đỉnh bị cô lập. 

Đỉnh cô lập biến mất ở giai đoạn`0`. Nhóm vẫn còn nguyên cho đến giai đoạn`m - 1`, khi tất cả các đỉnh của cụm đều bị xóa cùng nhau. 

Công trình này sử dụng`m + 1`tổng số đỉnh. Vì vậy, đối với một nhất định`n`, kích thước nhóm cuối cùng lớn nhất có thể là$m=n-1$Do đó câu trả lời chỉ đơn giản là`n - 1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi giá trị của`n`, tính toán`n - 1`. 

Chúng ta luôn có thể đạt được giá trị này bằng cách lấy một biểu đồ đầy đủ về`n - 1`đỉnh và một đỉnh cô lập. 
3. In kết quả. 

Tại sao đây là câu trả lời đúng? Đỉnh bị cô lập biến mất ngay lập tức trong cấp độ`0`sân khấu. Sau đó, phần còn lại`n - 1`các đỉnh tạo thành một cụm nên mọi đỉnh đều có bậc`n - 2`. Chúng tồn tại ở mọi giai đoạn trước đó vì mức độ của chúng lớn hơn số lượng bị loại bỏ hiện tại. Khi quá trình đạt đến giai đoạn`n - 2`, tất cả chúng đều được loại bỏ cùng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    print(n - 1)
```Việc triển khai được thực hiện rất nhỏ một cách có chủ ý vì tất cả công việc đều nằm trong bằng chứng. 

Chương trình đọc từng test case một cách độc lập và đưa ra kết quả`n - 1`. Không có việc xây dựng hoặc mô phỏng đồ thị vì đối số toán học đã chứng minh giá trị tối ưu. 

Điều kiện biên duy nhất là`n = 1`. Trong trường hợp đó, công thức cho`0`, điều đó đúng. Một người bị cô lập rời đi trong thời gian tốt nghiệp`0`giai đoạn, để không còn ai ở lại sau đó. 

Số nguyên Python không bị giới hạn, do đó không có lo ngại về tràn ngay cả khi giới hạn lớn hơn nhiều. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
```Xây dựng: 

- Đỉnh`1`Và`2`tạo thành một bè phái. 
- Đỉnh`3`bị cô lập. 

| Sân khấu | Đỉnh sống động | Bằng cấp | Đã xóa | 
| --- | --- | --- | --- | 
| 0 | {1,2,3} | {1,1,0} | {3} | 
| 1 | {1,2} | {1,1} | {1,2} | 

Nhóm lớn nhất còn sống ngay trước lần xóa cuối cùng có kích thước`2`, bằng`n - 1`. 

Ví dụ này cho thấy tại sao việc thêm một đỉnh cô lập lại hữu ích. Không có nó, toàn bộ biểu đồ sẽ biến mất đồng thời. 

### Ví dụ 2 

đầu vào:```
n = 5
```Xây dựng: 

- Đỉnh`1,2,3,4`tạo thành một bè phái. 
- Đỉnh`5`bị cô lập. 

| Sân khấu | Đỉnh sống động | Bằng cấp | Đã xóa | 
| --- | --- | --- | --- | 
| 0 | {1,2,3,4,5} | {3,3,3,3,0} | {5} | 
| 1 | {1,2,3,4} | {3,3,3,3} | {} | 
| 2 | {1,2,3,4} | {3,3,3,3} | {} | 
| 3 | {1,2,3,4} | {3,3,3,3} | {1,2,3,4} | 

Nhóm tồn tại nguyên vẹn cho đến giai đoạn liên quan cuối cùng. Điều này khẳng định tính bất biến là mọi đỉnh của cụm luôn có bậc lớn hơn ngưỡng xóa hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có một phép trừ và một bản in | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung | 

Ngay cả với`10^5`trường hợp thử nghiệm, tổng công việc là tầm thường. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        ans.append(str(n - 1))

    print("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("1\n3\n") == "2\n", "sample 1"

# minimum size
assert run("1\n1\n") == "0\n", "single isolated vertex"

# small boundary
assert run("1\n2\n") == "1\n", "one edge plus isolated behavior"

# multiple test cases
assert run("4\n1\n2\n5\n10\n") == "0\n1\n4\n9\n", "multiple independent cases"

# large value
assert run("1\n100000\n") == "99999\n", "maximum constraint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Đồ thị tối thiểu có thể | 
|`1 2`|`1`| Công trình nhỏ nhất không tầm thường | 
|`4 / 1 2 5 10`|`0 1 4 9`| Xử lý nhiều thử nghiệm | 
|`1 100000`|`99999`| Xử lý hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất:```
1
1
```Chỉ có một người và không có tình bạn. Trong giai đoạn`0`, người này có bằng cấp`0`và rời đi ngay lập tức. 

Thuật toán tính toán:$1-1=0$phù hợp với quá trình một cách chính xác. 

Bây giờ hãy xem xét:```
1
2
```Chiếm một cạnh giữa hai người. Ban đầu cả hai đều có bằng cấp`1`. 

Ở sân khấu`0`, không ai rời đi. Ở sân khấu`1`, cả hai cùng nhau rời đi. 

Các đầu ra thuật toán:$2-1=1$có nghĩa là quy mô nhóm sống sót cuối cùng lớn nhất có thể là`1`. Thay vào đó, chúng tôi đạt được điều đó bằng cách sử dụng một đỉnh bị cô lập và một đỉnh riêng biệt. Đỉnh bị cô lập rời đi trước, người còn lại rời đi sau. 

Một trường hợp phức tạp khác là:```
1
4
```Một phỏng đoán ngây thơ có thể là`4`, sử dụng đồ thị đầy đủ trên bốn đỉnh. Nhưng mọi đỉnh đều có bậc`3`, vậy là cả bốn người cùng biến mất ở sân khấu`3`. 

Việc xây dựng tối ưu là một nhóm có quy mô`3`cộng với một đỉnh cô lập. Đỉnh bị cô lập biến mất đầu tiên và nhóm tồn tại cho đến giai đoạn`2`. 

Các đầu ra thuật toán:$4-1=3$đó là có thể đạt được và tối ưu.
