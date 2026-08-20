---
title: "CF 102263B - Đường tới Arabella"
description: "Trò chơi bắt đầu với số hiện tại m = n. Kilani di chuyển đầu tiên. Ở mỗi lượt, người chơi trừ m một số nguyên dương x. Nếu m k, người chơi có thể trừ bất cứ thứ gì từ 1 đến m-k, do đó giá trị mới có thể là bất kỳ số nguyên nào từ k đến m-1."
date: "2026-08-19T02:52:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102263
codeforces_index: "B"
codeforces_contest_name: "ArabellaCPC 2019"
rating: 0
weight: 102263
solve_time_s: 98
verified: true
draft: false
---

[CF 102263B - Đường đến Arabella](https://codeforces.com/problemset/problem/102263/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi bắt đầu với số hiện tại`m = n`. Kilani di chuyển đầu tiên. Ở mỗi lượt, người chơi trừ một số nguyên dương`x`từ`m`. Nếu như`m > k`, người chơi có thể trừ đi bất cứ thứ gì từ`1`bởi vì`m-k`, vì vậy giá trị mới có thể là bất kỳ số nguyên nào từ`k`bởi vì`m-1`. Nếu như`m <= k`, phép trừ chỉ được phép`1`, vì vậy trò chơi tiếp tục bằng cách giảm dần số một. 

Người chơi phải đối mặt`m = 0`thua vì không có động thái hợp pháp. Đối với mỗi trường hợp thử nghiệm, chúng ta cần quyết định xem vị trí bắt đầu có`(n, k)`đang giành chiến thắng cho Kilani, giả sử cả hai người chơi đều lựa chọn tối ưu. Chúng tôi in`Kilani`để có vị trí xuất phát chiến thắng và`Ayoub`nếu không thì. 

Lớn nhất có thể`n`là`10^9`, trong khi có thể có tới`10^4`trường hợp thử nghiệm. Một thuật toán xử lý mọi giá trị lên đến`n`đã quá chậm vì một trường hợp thử nghiệm có thể yêu cầu hàng tỷ thao tác. Một thuật toán với công việc bậc hai trong`n`là hoàn toàn không thể. Giải pháp dự định phải khai thác cấu trúc của các bước di chuyển được phép và giảm từng trường hợp thử nghiệm về thời gian không đổi. 

Có một số trường hợp ranh giới trong đó việc coi trò chơi như phép trừ thông thường có thể dẫn đến một câu trả lời sai. Vì`n = 1, k = 1`, Kilani thắng. Động thái duy nhất là trừ`1`, khiến Ayoub bằng 0 và không thể di chuyển. Quy tắc chẵn lẻ bất cẩn được áp dụng cho phạm vi sai có thể phân loại sai vị trí này. 

Vì`n = 2, k = 2`, Kilani thua. Từ`m <= k`, hành động duy nhất là`2 -> 1`, và Ayoub sau đó di chuyển`1 -> 0`, khiến Kilani không thể di chuyển. Sự thật là`n = k`là một vấn đề ranh giới đặc biệt ở đây. 

Vì`n = 2, k = 1`, câu trả lời đúng là`Ayoub`. Kilani chỉ có thể di chuyển từ`2`ĐẾN`1`, sau đó Ayoub chuyển đến`0`. Đây là vị trí đầu tiên ngay trên số lẻ`k`, và nó đang thua mặc dù có một động thái hợp pháp. 

Vì`n = 3, k = 1`, câu trả lời là`Kilani`. Kilani có thể di chuyển trực tiếp từ`3`ĐẾN`1`, buộc Ayoub phải thực hiện nước đi cuối cùng về số 0. Điều này phân biệt`n = k+1`từ`n >= k+2`. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là phân loại mọi giá trị có thể có của`m`như thắng hay thua. Một vị trí sẽ thua nếu mọi nước đi hợp pháp đều đạt đến vị trí thắng. Sẽ thắng nếu có ít nhất một nước đi hợp lệ đạt đến thế thua. Vì`m <= k`, chỉ có một quá trình chuyển đổi,`m -> m-1`. Vì`m > k`, có`m-k`di chuyển có thể, đạt mọi giá trị từ`k`bởi vì`m-1`. 

Phương pháp quy hoạch động này đúng vì mỗi bước di chuyển đều giảm`m`, nên ta có thể xác định được vị trí theo thứ tự tăng dần. Tuy nhiên, việc tính toán trạng thái của một vị trí trên`k`có thể kiểm tra tới`m-k`các trạng thái trước đó. Tính toán tất cả các vị trí lên đến`n`do đó cần thời gian bậc hai,`O(n^2)`, trong trường hợp xấu nhất. Với`n = 10^9`, điều đó có nghĩa là đại khái`5 * 10^17`kiểm tra chuyển tiếp cho một trường hợp thử nghiệm lớn, điều này gần như không khả thi. 

Quan sát quan trọng là chúng ta không thực sự cần phải phân loại tất cả các vị trí. Đầu tiên hãy xem xét các vị trí tại và dưới`k`. Vì nước đi duy nhất là trừ một nên trò chơi chỉ đơn giản là một trò chơi ăn một thông thường. Như vậy`k`đang thua chính xác khi nào`k`là chẵn và thắng chính xác khi`k`thật kỳ quặc. 

Bây giờ hãy xem xét các giá trị lớn hơn`k`. Từ bất kỳ giá trị nào như vậy`m`, người chơi có thể di chuyển đến mọi giá trị trong khoảng`[k, m-1]`. Ngay khi khoảng thời gian này chứa một vị thế thua lỗ,`m`tự động thắng vì người chơi có thể di chuyển thẳng đến vị trí thua đó. 

Nếu như`k`thì chẵn`k`bản thân nó đang thua cuộc. Theo đó, mọi`m > k`đang thắng vì mọi vị trí như vậy đều có thể chuyển thẳng tới`k`. 

Nếu như`k`thế thì kỳ quặc`k`đang chiến thắng. Vị trí tiếp theo,`k+1`, chỉ có thể di chuyển đến`k`, thế là thua. Mỗi vị trí`m >= k+2`có thể di chuyển trực tiếp đến`k+1`, khiến tất cả đều chiến thắng. 

Điều này chỉ để lại hai vị trí xuất phát có thể bị mất. Khi`k`là chẵn, chỉ`n = k`đang thua. Khi`k`thật kỳ lạ, chỉ`n = k+1`đang thua. Mọi vị trí xuất phát khác đều mang lại chiến thắng cho Kilani. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`cho trường hợp thử nghiệm hiện tại. Chúng ta chỉ cần hai giá trị này vì trò chơi có đặc điểm dạng đóng về các vị trí thua của nó. 
2. Kiểm tra xem`k`là chẵn. Trong trường hợp này, vị trí`m = k`đang thua vì trò chơi dưới đây`k`chỉ bao gồm việc trừ một và một số chẵn các nước đi như vậy sẽ khiến người chơi di chuyển ở mức 0. 
3. Nếu`k`chẵn, phân loại`n = k`như thua và mọi`n > k`như chiến thắng. Mọi giá trị trên`k`có thể di chuyển trực tiếp đến vị trí thua cuộc`k`. 
4. Nếu`k`thật kỳ lạ, vị trí`k`đang chiến thắng, trong khi`k+1`đang thua vì đích đến duy nhất có thể có của nó là vị trí chiến thắng`k`. 
5. Đối với số lẻ`k`, phân loại`n = k+1`như thua và mọi thứ khác hợp lệ`n`như chiến thắng. Bất kỳ giá trị nào ít nhất`k+2`có thể di chuyển trực tiếp đến vị trí thua cuộc`k+1`. 
6. In`Ayoub`chính xác cho các trường hợp thua được mô tả ở trên. In`Kilani`cho tất cả các trường hợp khác. 

### Tại sao nó hoạt động 

Điều bất biến là mọi vị trí lớn hơn`k`có thể đạt đến mọi vị trí nhỏ hơn xuống`k`. Khi`k`là chẵn,`k`là vị trí thua đầu tiên, vì vậy nó khiến mọi vị trí lớn hơn đều thắng. Khi`k`thật kỳ lạ,`k`đang chiến thắng và`k+1`trở thành vị trí thua đầu tiên, sau đó khiến mọi vị trí phía trên nó thắng. Vì vậy, vị trí thua duy nhất là`k`thậm chí`k`, Và`k+1`kỳ quặc`k`. Thuật toán kiểm tra chính xác các vị trí đó, do đó nó tạo ra người chiến thắng chính xác cho mọi vị trí hợp lệ.`(n, k)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data: str) -> str:
    tokens = data.split()
    t = int(tokens[0])
    pos = 1
    ans = []

    for _ in range(t):
        n = int(tokens[pos])
        k = int(tokens[pos + 1])
        pos += 2

        if k % 2 == 0:
            losing = (n == k)
        else:
            losing = (n == k + 1)

        ans.append("Ayoub" if losing else "Kilani")

    return "\n".join(ans)

def main():
    data = sys.stdin.read()
    sys.stdout.write(solve(data))

if __name__ == "__main__":
    main()
```các`solve`hàm phân tích tất cả các trường hợp kiểm thử cùng một lúc và lưu trữ các câu trả lời vào danh sách trước khi nối chúng. Điều này tránh các hoạt động đầu ra lặp đi lặp lại và thuận tiện cho tối đa`10^4`trường hợp. 

Kiểm tra tính chẵn lẻ`k % 2 == 0`xác định mô hình nào trong hai mô hình vị trí thua lỗ có thể được áp dụng. Thậm chí`k`, chỉ một`n == k`đang thua. Đối với số lẻ`k`, chỉ một`n == k + 1`đang thua. 

Sự so sánh phải sử dụng sự bình đẳng hơn là sự bất bình đẳng. Ví dụ, với số lẻ`k = 1`, cả hai`n = 2`Và`n = 3`là khác nhau:`2`đang thua, trong khi`3`đã chiến thắng vì nó có thể di chuyển trực tiếp đến`2`. 

Số nguyên Python không có vấn đề tràn ở đây và phép toán số học lớn nhất chỉ`k + 1`, rất nhỏ so với phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét`n = 2, k = 1`. Từ`k`thật kỳ lạ, vị trí thua duy nhất ở trên`k`là`k+1 = 2`. 

| n | k | k chẵn lẻ | Mất vị trí | Kết quả | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | lẻ | 2 | Ayoub | 

Kilani bắt đầu lúc`2`. Bởi vì`m > k`, phép trừ tối đa là`1`, vì vậy cách duy nhất là`2 -> 1`. Ayoub lúc đó đang ở`1`, và hành động duy nhất là`1 -> 0`. Kilani đối mặt với con số 0 và thua cuộc. Do đó đầu ra là`Ayoub`. 

### Mẫu 2 

Hãy xem xét`n = 4, k = 1`. Lại`k`là kỳ lạ, vì vậy chỉ`k+1 = 2`đang thua. 

| n | k | k chẵn lẻ | Mất vị trí | Kết quả | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | lẻ | 2 | Kilani | 

Từ`4`, Kilani có thể trừ`2`, đạt`2`. Chức vụ`2`đang thua người chơi tiếp theo nên Ayoub phải chuyển từ`2`ĐẾN`1`, sau đó Kilani chuyển đến`0`. Như vậy Kilani có một chiến lược chiến thắng và kết quả đầu ra là`Kilani`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm yêu cầu một số phép tính số học không đổi. | 
| Không gian | O(T) | Chuỗi câu trả lời được lưu trữ trước khi được viết. | 

Với nhiều nhất`10^4`trường hợp kiểm thử, thuật toán chỉ thực hiện một số thao tác cho mỗi trường hợp, bất kể`n`là`10`hoặc`10^9`. Do đó nó tránh được sự phụ thuộc không thể có vào độ lớn của`n`và dễ dàng phù hợp với các ràng buộc đã nêu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(data: str) -> str:
    tokens = data.split()
    t = int(tokens[0])
    pos = 1
    ans = []

    for _ in range(t):
        n = int(tokens[pos])
        k = int(tokens[pos + 1])
        pos += 2

        if k % 2 == 0:
            losing = (n == k)
        else:
            losing = (n == k + 1)

        ans.append("Ayoub" if losing else "Kilani")

    return "\n".join(ans)

def run(inp: str) -> str:
    return solve(inp)

# Provided samples
assert run("2\n2 1\n4 1\n") == "Ayoub\nKilani", "provided samples"

# Minimum-size input
assert run("1\n1 1\n") == "Kilani", "n = k = 1"

# Even k at the exact losing position
assert run("1\n2 2\n") == "Ayoub", "even k and n = k"

# Odd k at the exact losing position n = k + 1
assert run("1\n6 5\n") == "Ayoub", "odd k and n = k + 1"

# Just above the losing position
assert run("1\n7 5\n") == "Kilani", "odd k and n = k + 2"

# Large boundary value
assert run("1\n1000000000 1000000000\n") == "Ayoub", "maximum n with even k"

# Multiple cases with different parities and boundaries
assert run(
    "4\n"
    "1 1\n"
    "2 1\n"
    "3 2\n"
    "4 2\n"
) == "Kilani\nAyoub\nKilani\nKilani", "mixed boundary cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1`|`Kilani`| Giá trị tối thiểu và số lẻ`k`với`n = k`| 
|`1 / 2 2`|`Ayoub`| Thậm chí`k`với vị trí thua chính xác | 
|`1 / 6 5`|`Ayoub`| Số lẻ`k`với`n = k + 1`| 
|`1 / 7 5`|`Kilani`| Vị trí đầu tiên sau vị trí thua cuộc | 
|`1 / 1000000000 1000000000`|`Ayoub`| Kích thước đầu vào tối đa và thậm chí`k`| 
| Đầu vào bốn trường hợp hỗn hợp |`Kilani / Ayoub / Kilani / Kilani`| Nhiều trường hợp và ranh giới chẵn lẻ | 

## Vỏ cạnh 

cho`n = k = 1`, thuật toán thấy rằng`k`là số lẻ và kiểm tra xem`n == k+1`, điều đó là sai. Nó trở lại`Kilani`. Trò chơi thực tế là`1 -> 0`, do đó người chơi bắt đầu sẽ thắng. 

Vì`n = k = 2`,`k`là số chẵn nên thuật toán sẽ kiểm tra xem`n == k`. Đó là sự thật và trở lại`Ayoub`. Trò chơi là`2 -> 1 -> 0`, do đó người chơi thứ hai thực hiện nước đi cuối cùng và giành chiến thắng. 

Vì`n = 2, k = 1`,`k`thật kỳ quặc và`n == k+1`, do đó thuật toán trả về`Ayoub`. Chỉ có một bước đi đầu tiên có thể xảy ra,`2 -> 1`, và Ayoub sau đó di chuyển`1 -> 0`, khiến Kilani không nhúc nhích. 

Vì`n = 3, k = 1`, kỳ lạ giống nhau-`k`quy tắc được áp dụng, nhưng bây giờ`n`lớn hơn`k+1`. Thuật toán trả về`Kilani`. Kilani có thể di chuyển trực tiếp từ`3`đến thế thua`2`, sau đó Ayoub bị buộc vào chuỗi thua. 

Đối với ranh giới tối đa`n = k = 10^9`,`k`là số chẵn và`n == k`, do đó thuật toán trả về`Ayoub`. Không có sự lặp lại tỷ lệ thuận với`n`xảy ra, đó chính xác là lý do tại sao giải pháp duy trì thời gian không đổi ngay cả ở giá trị đầu vào lớn nhất.
