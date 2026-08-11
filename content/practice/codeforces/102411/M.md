---
title: "CF 102411M - Quản lý khó khăn"
description: "Chúng tôi có một mảng mô tả những khó khăn của các vấn đề được công bố trong những ngày liên tiếp. Chúng ta cần chọn ba chỉ số i < j < k sao cho độ khó ở giữa nằm chính xác giữa hai chỉ số còn lại: [ aj-ai=ak-aj. ] Sắp xếp lại sẽ có [ ai+ak=2aj."
date: "2026-08-12T00:23:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102411
codeforces_index: "M"
codeforces_contest_name: "ICPC 2019-2020 North-Western Russia Regional Contest"
rating: 0
weight: 102411
solve_time_s: 104
verified: true
draft: false
---

[CF 102411M - Quản lý khó khăn](https://codeforces.com/problemset/problem/102411/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`a`mô tả những khó khăn của các vấn đề được công bố liên tục trong nhiều ngày. Chúng ta cần chọn ba chỉ số`i < j < k`sao cho độ khó ở giữa chính xác là một nửa giữa hai độ khó còn lại: 

[ 
a_j-a_i=a_k-a_j. 
] 

Sắp xếp lại mang lại 

[ 
a_i+a_k=2a_j. 
] 

Vì vậy, nhiệm vụ là đếm từng bộ ba chỉ số có ba độ khó tạo thành một cấp số cộng theo thứ tự mảng ban đầu của chúng. Bản thân các giá trị không cần phải khác biệt và hiệu số chung có thể là dương, 0 hoặc âm. 

Có nhiều nhất`n = 2000`các vị trí. Một thuật toán khối kiểm tra khoảng 

[ 
\binom{2000}{3}=1,331,334,000 
] 

gấp ba lần trong trường hợp thử nghiệm lớn nhất. Với tối đa mười trường hợp thử nghiệm và giới hạn hai giây, khối lượng công việc đó vượt xa những gì Python có thể xử lý. Mặt khác, một nghiệm bậc hai thực hiện khoảng 

[ 
\binom{2000}{2}=1.999.000 
] 

các lần lặp bên trong cho mỗi trường hợp thử nghiệm, điều này rất thực tế. 

Các giá trị có thể lớn bằng (10^9), do đó biểu thức`2 * a[j] - a[k]`có thể đạt tới độ lớn khoảng (2\cdot10^9). Số nguyên Python xử lý việc này một cách trực tiếp, trong khi các ngôn ngữ có loại số nguyên có chiều rộng cố định nên sử dụng loại có dấu đủ rộng. Giá trị của câu trả lời có thể vượt quá (10^9), đạt tới (1.331.334.000), do đó, số nguyên có dấu 32 bit vẫn đủ ở đây, mặc dù số nguyên 64 bit là lựa chọn thông thường. 

Một trường hợp tinh tế là các giá trị lặp lại. Ví dụ, với`a = [5, 5, 5, 5]`, mọi lựa chọn trong số ba vị trí đều hiệu quả, vì vậy câu trả lời là 4. Việc triển khai bất cẩn chỉ lưu trữ liệu một giá trị có tồn tại hay không, thay vì số lần nó xuất hiện, sẽ làm mất tính đa bội và tính thiếu. 

Một trường hợp khác là hiệu chung bằng 0 hoặc âm. Vì`a = [30, 20, 10]`, bộ ba duy nhất có sự khác biệt`-10`Và`-10`, vì vậy câu trả lời là 1. Một cách tiếp cận giả định mảng phải tăng hoặc chỉ kiểm tra chênh lệch dương, sẽ trả về 0 không chính xác. 

Trường hợp thứ ba là khi giá trị bắt buộc không tồn tại trong tiền tố. Vì`a = [1, 1, 2]`, chiếm vị trí ở giữa mang lại`2 * 1 - 2 = 0`, nhưng không có số 0 sớm hơn, vì vậy câu trả lời là 0. Việc tra cứu phải cho phép các giá trị đích tùy ý và chỉ trả về 0 khi không có mục tiêu. 

Cuối cùng, thứ tự của các chỉ số rất quan trọng. Vì`a = [1, 2, 1]`, các giá trị là một tập hợp chứa cấp số cộng, nhưng bộ ba duy nhất có thể có khác biệt`1`Và`-1`, vì vậy câu trả lời là 0. Việc đếm các kết hợp giá trị mà không tôn trọng thứ tự chỉ mục ban đầu sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là liệt kê mọi bộ ba có thể`i < j < k`và kiểm tra xem`a[i] + a[k] == 2 * a[j]`. Điều này đúng vì mỗi câu trả lời hợp lệ là một bộ ba như vậy và mỗi bộ ba thỏa mãn phương trình được tính chính xác một lần. Vấn đề là số lượng gấp ba. Vì`n = 2000`, có chính xác là 1.331.334.000 trong số đó, khiến cho việc liệt kê khối trở nên quá chậm. 

Quan sát hữu ích đến từ việc sửa chỉ số ở giữa`j`. Một lần`j`và điểm cuối phù hợp`k`được cố định, giá trị bên trái được yêu cầu không còn là ẩn số. Từ 

[ 
a_i+a_k=2a_j 
] 

chúng tôi có được 

[ 
a_i=2a_j-a_k. 
] 

Vì vậy với mỗi vị trí`k > j`, chúng ta chỉ cần biết có bao nhiêu vị trí trước đó`i < j`chứa giá trị`2 * a[j] - a[k]`. 

Điều này biến vấn đề thành vấn đề tra cứu tần số. Trong khi xử lý các vị trí từ trái sang phải, hãy duy trì bản đồ tần số chứa chính xác các giá trị tại các vị trí trước vị trí ở giữa hiện tại. Đối với mỗi khả năng`k`ở bên phải, tính giá trị cần tìm ở bên trái và cộng tần số của nó vào đáp án. 

Lực lượng vũ phu hoạt động vì nó kiểm tra rõ ràng mọi bộ ba có thể, nhưng không thành công vì có quá nhiều bộ ba. Quan sát rằng một điểm cuối có thể được xác định từ hai điểm cuối còn lại cho phép chúng ta thay thế vòng lặp trong cùng bằng tra cứu bản đồ băm trung bình (O(1)). Chúng tôi vẫn xem xét mọi cặp liên quan đến vị trí ở giữa, đưa ra thuật toán (O(n^2)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n²) dự kiến ​​| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo bản đồ tần số trống`freq`. Nó sẽ lưu trữ số lần mỗi khó khăn đã xuất hiện trước vị trí chính giữa hiện tại. 
2. Lặp lại chỉ số ở giữa có thể`j`từ trái sang phải. Khi bắt đầu lần lặp này,`freq`chứa chính xác các phần tử có chỉ số nhỏ hơn`j`. 
3. Đối với mọi vị trí`k > j`, tính toán 

[ 
mục tiêu=2a_j-a_k. 
] 

Bất kỳ chỉ mục bên trái hợp lệ nào`i`phải thỏa mãn`a[i] = target`, vì vậy số lượng lựa chọn hợp lệ cho trường hợp cụ thể này`j`Và`k`chính xác là`freq[target]`. 
4. Thêm`freq[target]`để có câu trả lời cho mọi`k`bên phải. Tra cứu tần số đếm tất cả các chỉ số bên trái hợp lệ cùng một lúc, bao gồm các giá trị lặp lại ở các vị trí khác nhau. 
5. Sau khi xử lý xong mọi vị trí bên phải, hãy chèn`a[j]`vào trong`freq`. Nó chỉ được chèn sau khi xử lý`j`, bởi vì`j`không được phép đóng vai trò là điểm cuối bên trái của chính nó cho điểm cuối bên phải sau này trong cùng một lần lặp. 
6. Sau khi tất cả các vị trí ở giữa đã được xử lý, hãy xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Bất biến là ngay trước khi xử lý chỉ số ở giữa`j`,`freq[x]`bằng số lượng chỉ số`i < j`vì cái gì`a[i] = x`. Đối với mọi`k > j`, điều kiện cấp số cộng tương đương với`a[i] = 2a[j] - a[k]`. Thuật toán cộng chính xác số chỉ số trước đó, do đó, mọi bộ ba hợp lệ có chỉ số ở giữa`j`được tính. Vì mỗi bộ ba có đúng một chỉ số ở giữa nên không có bộ ba nào được tính nhiều hơn một lần. Sau khi xử lý`j`, thêm`a[j]`khôi phục bất biến cho chỉ số ở giữa tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    answers = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = {}
        ans = 0

        for j in range(n):
            middle = a[j]

            for k in range(j + 1, n):
                target = 2 * middle - a[k]
                ans += freq.get(target, 0)

            freq[middle] = freq.get(middle, 0) + 1

        answers.append(str(ans))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Vòng lặp bên ngoài chọn chỉ số ở giữa`j`, phù hợp với bước đầu tiên của thuật toán. Trước khi vòng lặp bắt đầu,`freq`trống vì không có vị trí nào trước chỉ số 0. 

Đối với một cố định`j`, vòng lặp bên trong sẽ ghé thăm mỗi`k > j`. biểu hiện`2 * middle - a[k]`chính xác là giá trị trước đó`a[i]`phải có để bộ ba tạo thành một cấp số cộng.`freq.get(target, 0)`đưa ra số lượng các vị trí trước đó, vì vậy việc thêm nó sẽ tính tất cả các lựa chọn tương thích của`i`cùng một lúc. 

Bản cập nhật tới`freq`xảy ra sau vòng lặp bên trong. Thứ tự này là cần thiết vì`freq`phải đại diện cho các chỉ số hoàn toàn nhỏ hơn`j`. Nếu như`a[j]`được chèn trước, vị trí ở giữa hiện tại có thể được tính không chính xác là điểm cuối bên trái. 

Phạm vi`range(j + 1, n)`thi hành`k > j`, trong khi bản đồ tần số đã có hiệu lực`i < j`. Do đó, thứ tự chặt chẽ của cả ba chỉ số được xây dựng trực tiếp vào cấu trúc lặp. 

Từ điển của Python cung cấp tính năng chèn và tra cứu (O(1)) dự kiến, cung cấp thời gian chạy dự kiến ​​theo phương pháp bậc hai. Số nguyên Python cũng tránh tràn khi tính toán`2 * middle - a[k]`hoặc lưu trữ câu trả lời. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp thử nghiệm mẫu đầu tiên,`a = [1, 2, 1, 2, 1]`. Bảng sau ghi lại mọi điểm cuối bên phải được kiểm tra.`freq`được hiển thị trước khi xử lý chỉ mục ở giữa đó. 

|`j`|`a[j]`|`freq`trước`j`|`k`|`a[k]`|`target`| Đã thêm | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 |`{}`| 1 | 2 | 0 | 0 | 0 | 
| 0 | 1 |`{}`| 2 | 1 | 1 | 0 | 0 | 
| 0 | 1 |`{}`| 3 | 2 | 0 | 0 | 0 | 
| 0 | 1 |`{}`| 4 | 1 | 1 | 0 | 0 | 
| 1 | 2 |`{1: 1}`| 2 | 1 | 3 | 0 | 0 | 
| 1 | 2 |`{1: 1}`| 3 | 2 | 2 | 0 | 0 | 
| 1 | 2 |`{1: 1}`| 4 | 1 | 3 | 0 | 0 | 
| 2 | 1 |`{1: 1, 2: 1}`| 3 | 2 | 0 | 0 | 0 | 
| 2 | 1 |`{1: 1, 2: 1}`| 4 | 1 | 1 | 1 | 1 | 
| 3 | 2 |`{1: 2, 2: 1}`| 4 | 1 | 3 | 0 | 1 | 

Tại`j = 2`Và`k = 4`, giá trị bên trái được yêu cầu là`1`. Có chính xác một lần xuất hiện trước đó của`1`, tại chỉ số 0, do đó bộ ba`(0, 2, 4)`được tính. Mọi tra cứu khác đều trả về 0, đưa ra câu trả lời mẫu là 1. Dấu vết cũng cho thấy lý do tại sao cần phải có tần số chứ không phải một tập hợp. 

Đối với trường hợp thử nghiệm mẫu thứ hai,`a = [30, 20, 10]`, chỉ có thể có một chỉ số ở giữa. 

|`j`|`a[j]`|`freq`trước`j`|`k`|`a[k]`|`target`| Đã thêm | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 30 |`{}`| 1 | 20 | 40 | 0 | 0 | 
| 0 | 30 |`{}`| 2 | 10 | 50 | 0 | 0 | 
| 1 | 20 |`{30: 1}`| 2 | 10 | 30 | 1 | 1 | 

Khi`j = 1`Và`k = 2`, mục tiêu là`30`và tiền tố chứa chính xác một`30`. Bộ ba kết quả là`(0, 1, 2)`, tương ứng với những khó khăn`30, 20, 10`. Điều này khẳng định rằng phương pháp này đương nhiên ủng hộ sai phân chung âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) dự kiến ​​| Mỗi cặp`(j, k)`với`j < k`được kiểm tra một lần và mỗi thao tác từ điển được mong đợi là O(1). | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi độ khó riêng biệt trong tiền tố. | 

Vì`n = 2000`, vòng lặp bên trong thực hiện tối đa 1.999.000 lần lặp cho một trường hợp thử nghiệm. Ngay cả với mười trường hợp thử nghiệm ở kích thước tối đa, con số này vẫn là khoảng 20 triệu lượt tra cứu từ điển, phù hợp với giới hạn hai giây trong một lần gửi Python được tối ưu hóa điển hình. Việc sử dụng bộ nhớ nhiều nhất là tuyến tính trong`n`, thấp hơn nhiều so với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    t = int(next(it))
    answers = []

    for _ in range(t):
        n = int(next(it))
        a = [int(next(it)) for _ in range(n)]

        freq = {}
        ans = 0

        for j in range(n):
            middle = a[j]

            for k in range(j + 1, n):
                target = 2 * middle - a[k]
                ans += freq.get(target, 0)

            freq[middle] = freq.get(middle, 0) + 1

        answers.append(str(ans))

    return "\n".join(answers)

def run(inp: str) -> str:
    return solution(inp)

sample = """\
4
5
1 2 1 2 1
3
30 20 10
5
1 2 2 3 4
9
3 1 4 1 5 9 2 6 5
"""

assert run(sample) == "1\n1\n4\n5", "provided samples"

assert run("""\
1
3
1 2 3
""") == "1", "minimum-size arithmetic progression"

assert run("""\
1
4
5 5 5 5
""") == "4", "all equal values"

assert run("""\
1
3
1 2 1
""") == "0", "ordering and sign of differences"

assert run("""\
1
2000
""" + " ".join(["1"] * 2000) + "\n") == "1331334000", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 2 3`|`1`| Kích thước đầu vào tối thiểu và cấp số sai phân dương cơ bản | 
|`4 / 5 5 5 5`|`4`| Giá trị trùng lặp và đếm tần số | 
|`3 / 1 2 1`|`0`| Thứ tự chỉ số gốc và sai phân thứ hai âm | 
|`n = 2000`, tất cả các giá trị`1`|`1331334000`| Kích thước tối đa, câu trả lời lớn và hiệu suất bậc hai | 

## Vỏ cạnh 

Đối với trường hợp giá trị lặp lại, hãy xem xét`a = [5, 5, 5, 5]`. Mỗi một trong bốn bộ ba có thể có đều hợp lệ, do đó kết quả đầu ra là 4. Khi xử lý`j = 2`, ví dụ: bản đồ tần số chứa hai lần xuất hiện trước đó của`5`. Điểm cuối bên phải`k = 3`yêu cầu`target = 2 * 5 - 5 = 5`, do đó thuật toán cộng thêm 2 cho hai điểm cuối bên trái có thể có. Các lần lặp lại trước đó đóng góp hai bộ ba còn lại. Câu trả lời cuối cùng chính xác là 4. 

Đối với sự khác biệt chung âm, hãy xem xét`a = [30, 20, 10]`. Tại`j = 1`, điểm cuối bên phải có giá trị 10, cho`target = 40 - 10 = 30`. Tiền tố chứa một số 30, do đó thuật toán thêm một số. Câu trả lời là 1. Không có giả định nào về việc tăng giá trị ở bất kỳ đâu trong quá trình tính toán. 

Đối với một mục tiêu vắng mặt, hãy xem xét`a = [1, 1, 2]`. Tại`j = 1`, mục tiêu cho`k = 2`là`2 * 1 - 2 = 0`. Bản đồ tần số chỉ chứa`{1: 1}`, Vì thế`freq.get(0, 0)`trả về số không. Thuật toán tạo ra 0, loại bỏ chính xác bộ ba duy nhất có thể. 

Đối với trường hợp nhạy cảm với thứ tự, hãy xem xét`a = [1, 2, 1]`. Tại`j = 1`,`k = 2`yêu cầu giá trị bên trái là`2 * 2 - 1 = 3`. Tiền tố chỉ chứa`1`, nên không có gì được tính. Câu trả lời là 0. Mặc dù các giá trị chứa`1, 2, 1`, hiệu liên tiếp của chúng là`1`Và`-1`, vì vậy chúng không tạo thành cấp số cộng cần thiết. 

Trường hợp có câu trả lời tối đa là`n = 2000`với mọi giá trị đều bằng`1`. Mọi lựa chọn trong ba chỉ số đều có tác dụng, mang lại 

[ 
\binom{2000}{3}=1.331.334.000. 
] 

Thuật toán đếm các kết hợp này thông qua tần số thay vì liệt kê chúng riêng lẻ, đó chính xác là lý do tại sao phương pháp bậc hai vẫn nhanh ngay cả khi số lượng bộ ba hợp lệ lên tới hơn một tỷ.
