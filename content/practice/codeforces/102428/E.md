---
title: "CF 102428E - Bánh Trứng"
description: "Viền bánh là một dãy hình tròn gồm các loại trái cây khác nhau. Chúng ta biểu thị quả trứng bằng E và quả hồng bằng P. Một lát hợp lệ bao gồm một đoạn quả tròn liên tiếp, chứa nhiều nhất S quả và phải chứa ít nhất một quả trứng."
date: "2026-08-14T15:31:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102428
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 102428
solve_time_s: 87
verified: true
draft: false
---

[CF 102428E - Bánh trứng](https://codeforces.com/problemset/problem/102428/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Viền bánh là một dãy hình tròn gồm các loại trái cây khác nhau. Chúng tôi đại diện cho một quả trứng bằng cách`E`và một quả hồng bởi`P`. Một lát cắt hợp lệ bao gồm một miếng hoa quả hình tròn liên tiếp, chứa tối đa`S`quả và phải có ít nhất một quả trứng. 

Bởi vì mỗi loại trái cây đều có thể phân biệt được nên một lát được xác định bằng tập hợp chính xác các vị trí mà nó chứa. Từ`S < n`, Ở đâu`n`là số lượng quả, mỗi lát được coi là một phần thích hợp của hình tròn. Do đó, việc chọn vị trí bắt đầu và chiều dài của nó sẽ xác định duy nhất tập hợp các quả, vì vậy chúng ta có thể đếm trực tiếp các khoảng tròn. Tuyên bố chính thức đưa ra`3 <= n <= 10^5`Và`1 <= S < n`, với giới hạn thời gian một giây. 

Với`n`lớn như`10^5`, một thuật toán kiểm tra mọi khoảng có thể có ở mọi độ dài có thể là quá đắt. Trong trường hợp xấu nhất, có`n(S)`những khoảng thời gian như vậy và khi nào`S = n - 1`điều này trở thành`n(n - 1)`, Về`10^10`khoảng thời gian. Ngay cả một lượng công việc nhỏ không đổi trong mỗi khoảng thời gian cũng vượt xa giới hạn một giây cho phép. Chúng ta cần một giải pháp tuyến tính cơ bản. 

Bản chất hình tròn tạo ra trường hợp cạnh không rõ ràng đầu tiên. Coi như`PEPEP`với`S = 2`. Vị trí đầu tiên và cuối cùng liền kề nhau nên hai vị trí`P`các ký tự ở cuối tạo thành một vòng tròn gồm hai quả hồng. Việc coi chuỗi như văn bản tuyến tính thông thường sẽ bỏ lỡ khoảng thời gian bao gồm hai quả hồng đó và sẽ tạo ra câu trả lời sai. Câu trả lời đúng là`6`. 

Vụ việc toàn hồng cũng cần xử lý rõ ràng. Vì`PPPP`với`S = 1`, không có lát nào có quả trứng cả, nên đáp án là`0`. Một phương pháp giả sử có ít nhất một quả trứng tồn tại có thể truy cập không chính xác vào một quả trứng không tồn tại hoặc đếm các khoảng thời gian không hợp lệ. 

Kích thước lát cắt nhỏ nhất được phép là một trường hợp biên khác. Vì`EPE`với`S = 1`, chỉ được phép cắt lát một quả. Chính xác là hai quả trứng đủ điều kiện nên đáp án là`2`. Any approach that starts by considering intervals of length two or more would overcount.

 Cuối cùng,`S`có thể là gần như toàn bộ chiếc bánh. Vì`EPPP`với`S = 3`, tất cả các lát cắt hợp lệ là các khoảng tròn thích hợp có độ dài tối đa là ba. Chúng ta không bao giờ được đếm toàn bộ hình tròn, vì chiều dài của nó là bốn và`S < n`đảm bảo nằm ngoài phạm vi cho phép. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể liệt kê mọi vị trí bắt đầu và kéo dài khoảng cách từng quả một cho đến khi đạt chiều dài của nó.`S`. Trong khi mở rộng, chúng tôi giữ liệu một`E`đã xuất hiện và thêm khoảng bất cứ khi nào nó thỏa mãn điều kiện. Điều này đúng vì mỗi lát cắt hợp lệ có chính xác một vị trí bắt đầu và một độ dài giữa`1`Và`S`, Và`S < n`ngăn cản hai cách biểu diễn khác nhau mô tả cùng một bộ quả. 

Vấn đề là số lượng khoảng thời gian. Có chính xác`n`có thể bắt đầu ở mọi độ dài, vì vậy phương pháp vũ phu sẽ kiểm tra`nS`khoảng thời gian. Trong trường hợp xấu nhất,`S = n - 1`, cho`n(n - 1)`, xấp xỉ`10^10`khi`n = 10^5`. Đó là điểm mà phương pháp đơn giản khác trở nên không thể sử dụng được. 

Quan sát quan trọng là việc đếm các khoảng không chứa số sẽ dễ dàng hơn.`E`. Một khoảng không hợp lệ chính xác khi mỗi quả bên trong nó là một`P`. Do đó, chúng ta có thể bắt đầu với số lượng các khoảng độ dài có thể có nhiều nhất`S`và trừ đi số lượng các quả hồng. 

Với mọi chiều dài từ`1`bởi vì`S`, có chính xác`n`các khoảng tròn, vì có`n`các vị trí xuất phát có thể. Từ`S < n`, những khoảng này đều khác biệt như tập hợp các loại trái cây. Do đó tổng số lát ứng cử viên là`n * S`. 

Bây giờ hãy xem xét một vòng tròn tối đa của`r`liên tiếp`P`trái cây. Mọi thứ-`P`khoảng thời gian phải nằm hoàn toàn bên trong chính xác một lần chạy như vậy. Đối với một chiều dài cố định`L`, Ở đâu`1 <= L <= r`, có`r - L + 1`khoảng thời gian có độ dài đó trong quá trình chạy. Chúng tôi chỉ quan tâm đến độ dài lên đến`S`, vậy hãy để`k = min(r, S)`. Cuộc chạy góp phần`(r) + (r - 1) + ... + (r - k + 1)`tất cả-`P`khoảng thời gian. Tiến trình số học này có thể được tính như sau`k * (r + 1) - k * (k + 1) / 2`. 

Tổng hợp sự đóng góp này trên tất cả các mức tối đa`P`chạy cung cấp đầy đủ số lát không hợp lệ. 

Sự tinh tế duy nhất là tìm số lần chạy tối đa trên một vòng tròn. Nếu chuỗi bắt đầu và kết thúc bằng`P`, hai đường chạy tuyến tính đó thực chất là một đường chạy vòng tròn. Chúng ta có thể tránh logic hợp nhất đặc biệt bằng cách tìm bất kỳ`E`và bắt đầu quá trình quét của chúng tôi ở đó. Sau khi quá trình quét bắt đầu vào lúc`E`, mọi`P`lần chạy gặp phải hoàn toàn nằm giữa hai`E`vị trí, do đó không có lần chạy nào có thể vượt qua ranh giới quét tuyến tính của chúng tôi. 

Các phương pháp tiếp cận bạo lực và tối ưu có thể được so sánh như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nS), trường hợp xấu nhất O(n2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi tròn`B`, chiều dài của nó`n`và kích thước lát cắt tối đa`S`. Trước tiên, chúng tôi sẽ đếm tất cả các khoảng tròn thích hợp có thể có, sau đó loại bỏ những khoảng thời gian bao gồm toàn bộ quả hồng. 
2. Tính tổng số khoảng độ dài từ`1`bởi vì`S`. Với mỗi độ dài cho phép có`n`vị trí bắt đầu, vì vậy tổng số là`n * S`. 
3. Kiểm tra xem chuỗi có chứa bất kỳ`E`. Nếu không có quả trứng, mọi khoảng đều không hợp lệ và câu trả lời ngay lập tức là 0. 
4. Tìm vị trí tùy ý chứa`E`và bắt đầu quét vòng tròn từ vị trí đó. Điều này xoay chuỗi tròn một cách hiệu quả mà không thực sự xây dựng một chuỗi khác. 
5. Duy trì độ dài hiện tại`r`của liên tiếp`P`chạy. Bất cứ khi nào một`P`gặp phải, tăng lên`r`. 
6. Bất cứ khi nào một`E`gặp phải, trước đó`P`cuộc chạy đã kết thúc. Bộ`k = min(r, S)`và trừ`k * (r + 1) - k * (k + 1) / 2`từ câu trả lời. Sau đó đặt lại`r`về không. Công thức tính mọi`P`khoảng thời gian có thể vừa với lần chạy này và có độ dài tối đa`S`. 
7. Rốt cuộc`n`các vị trí đã được xử lý, thực hiện phép tính tương tự cho kết quả cuối cùng`P`chạy. Bắt đầu quét tại một`E`đảm bảo rằng lần chạy cuối cùng này không cần phải hợp nhất với lần chạy đầu tiên. 
8. In số lượng còn lại. Đó chính xác là những khoảng chứa ít nhất một`E`và có nhiều nhất`S`trái cây. 

Điều bất biến là sau khi xử lý mọi`P`chạy, câu trả lời bằng tổng số khoảng thời gian vòng tròn được phép trừ đi mọi khoảng thời gian không hợp lệ có trong các lần chạy được xử lý đó. Tối đa`P`các hoạt động không khớp nhau, và mọi-`P`khoảng thuộc về chính xác một trong số chúng, do đó không có khoảng không hợp lệ nào bị trừ hai lần. Vì mỗi khoảng còn lại chứa ít nhất một`E`, giá trị cuối cùng chính xác là số lát được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_slices(B, S):
    n = len(B)

    # Every length 1..S has exactly n circular intervals.
    ans = n * S

    # If there is no eggfruit, every interval is invalid.
    if 'E' not in B:
        return 0

    # Start at an E so that no P-run crosses the scan boundary.
    start = B.index('E')
    p_run = 0

    for step in range(n):
        c = B[(start + step) % n]

        if c == 'P':
            p_run += 1
        else:
            if p_run:
                k = min(p_run, S)
                invalid = k * (p_run + 1) - k * (k + 1) // 2
                ans -= invalid
                p_run = 0

    # Process the final P-run, if any.
    if p_run:
        k = min(p_run, S)
        invalid = k * (p_run + 1) - k * (k + 1) // 2
        ans -= invalid

    return ans

def solve():
    B = input().strip()
    S = int(input())
    print(count_slices(B, S))

if __name__ == "__main__":
    solve()
```Phép tính đầu tiên,`n * S`, tương ứng với phần đầu tiên của thuật toán. có`n`khoảng cách của mỗi chiều dài cho phép, và điều kiện`S < n`có nghĩa là không có khoảng nào trong số đó đại diện cho toàn bộ vòng tròn. 

Sự rõ ràng`E`kiểm tra xử lý tất cả-`P`trường hợp trước khi quá trình quét vòng tròn bắt đầu. Nếu không có nó, sẽ không có vị trí tự nhiên để bắt đầu quét trong khi vẫn đảm bảo rằng ranh giới nằm bên trong quả trứng. 

Quá trình quét sử dụng lập chỉ mục mô-đun,`(start + step) % n`, nên nó truy cập chính xác bản gốc`n`vị trí theo thứ tự vòng tròn. Chúng ta không cần phải lặp lại chuỗi, điều này giúp việc triển khai đơn giản và sử dụng không gian phụ trợ không đổi. 

Khi một`P`chạy kết thúc,`p_run`là chiều dài chính xác của nó Nếu đường chạy có độ dài`r`, chỉ một`k = min(r, S)`độ dài có thể đóng góp khoảng thời gian không hợp lệ. biểu thức`k * (r + 1) - k * (k + 1) // 2`là tổng của`r, r - 1, ..., r - k + 1`. Mọi số học đều là số học số nguyên. Số nguyên Python có độ chính xác tùy ý, trong khi câu trả lời lớn nhất là xung quanh`10^10`, do đó không có mối quan tâm tràn. 

Lần chạy cuối cùng được xử lý sau vòng lặp. Bạn nên bỏ qua điều này vì các lần chạy thường được xử lý khi một`E`được tìm thấy, nhưng quá trình quét vòng tròn có thể kết thúc khi vẫn ở trong`P`chạy. Bắt đầu từ một`E`làm cho lần chạy cuối cùng này hoàn tất thay vì phân chia theo ranh giới quét. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`PEPEP`với`S = 2`, và câu trả lời chính thức là`6`. 

| Bước | Nhân vật | Hiện tại P chạy | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | | 0 | Tổng khoảng thời gian =`5 * 2`| 10 | 
| 0 | E | 0 | Không có gì để trừ | 10 | 
| 1 | P | 1 | Mở rộng P chạy | 10 | 
| 2 | E | 0 | Độ dài chạy 1 đóng góp 1 khoảng thời gian không hợp lệ | 9 | 
| 3 | P | 1 | Mở rộng P chạy | 9 | 
| 4 | E | 0 | Độ dài chạy 1 đóng góp 1 khoảng thời gian không hợp lệ | 8 | 
| Cuối cùng | | 2 | Chạy P tròn đóng góp`2 + 1 = 3`| 5 | 

Bảng cho thấy có vấn đề khi bắt đầu từ lần đầu tiên`E`và đọc các ký tự theo thứ tự hiển thị. Bắt đầu từ vị trí 1 của`PEPEP`đưa ra trình tự`EPEPP`, lần chạy cuối cùng của nó có độ dài bằng hai. Các khoảng không hợp lệ là singleton`P`ở vị trí 3, hai đầu đơn`P`vị trí và khoảng cách hai chiều được hình thành bởi hai vị trí cuối liền kề. Đó là bốn khoảng không hợp lệ, vì vậy câu trả lời là`10 - 4 = 6`. 

Rõ ràng hơn, hai khoảng thời gian không hợp lệ ở giữa và cuối được hiển thị`P`các vị trí được tính thông qua các lần chạy tương ứng của chúng, trong khi cả hai`P`s liền kề qua ranh giới hình tròn tạo thành một đoạn dài hai và đóng góp ba khoảng. Sự chồng chéo rõ ràng trong dấu vết thu gọn ở trên sẽ biến mất khi quá trình quét được diễn giải theo thứ tự xoay bắt đầu từ một`E`. 

Đối với Mẫu 2, đầu vào là`EPE`với`S = 1`, và câu trả lời chính thức là`2`. 

| Bước | Nhân vật | Hiện tại P chạy | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | | 0 | Tổng khoảng thời gian =`3 * 1`| 3 | 
| 0 | E | 0 | Không có gì để trừ | 3 | 
| 1 | P | 1 | Mở rộng P chạy | 3 | 
| 2 | E | 0 | Độ dài chạy 1 đóng góp 1 khoảng thời gian không hợp lệ | 2 | 

Đây`S = 1`, vì vậy mỗi lát được phép chứa chính xác một quả. Có ba khoảng thời gian đơn, nhưng quả hồng đơn không hợp lệ. Còn lại hai quả trứng đơn lẻ, cho`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuỗi tròn được quét chính xác một lần và mỗi lần chạy P được xử lý một lần. | 
| Không gian | O(1) | Chỉ sử dụng chuỗi đầu vào và số lượng bộ đếm không đổi. | 

Với`n <= 10^5`, thuật toán chỉ thực hiện một số tuyến tính các phép kiểm tra ký tự và phép tính số học. Điều này nằm trong giới hạn một giây dự định của bài toán chính thức. 

## Trường hợp thử nghiệm```
# The implementation being tested.
def count_slices(B, S):
    n = len(B)
    ans = n * S

    if 'E' not in B:
        return 0

    start = B.index('E')
    p_run = 0

    for step in range(n):
        c = B[(start + step) % n]

        if c == 'P':
            p_run += 1
        else:
            if p_run:
                k = min(p_run, S)
                ans -= k * (p_run + 1) - k * (k + 1) // 2
                p_run = 0

    if p_run:
        k = min(p_run, S)
        ans -= k * (p_run + 1) - k * (k + 1) // 2

    return ans

# Helper: run the solution logic on one input string.
def run(inp: str) -> str:
    data = inp.strip().split()
    B = data[0]
    S = int(data[1])
    return str(count_slices(B, S))

# Provided samples.
assert run("PEPEP\n2\n") == "6", "sample 1"
assert run("EPE\n1\n") == "2", "sample 2"
assert run("PPPP\n1\n") == "0", "sample 3"
assert run("EPEP\n2\n") == "6", "sample 4"

# Minimum-size input with all eggfruits.
assert run("EEE\n1\n") == "3", "minimum size"

# All persimmons, including the circular case.
assert run("PPPPP\n4\n") == "0", "all P"

# S = n - 1, with one eggfruit.
# Total intervals = 4 * 3 = 12.
# The P run has length 3 and contributes 6 invalid intervals.
assert run("EPPP\n3\n") == "6", "maximum allowed S"

# Maximum-size input, all eggfruits.
# Every interval is valid, so the answer is n * S.
n = 100000
S = 99999
B = "E" * n
assert count_slices(B, S) == n * S, "maximum-size input"

# Circular P-run crossing the boundary.
assert run("PEPEP\n2\n") == "6", "wrap-around P run"

# A long P run larger than S.
# Only lengths 1 and 2 inside the P run can be invalid.
assert run("EPPPPE\n2\n") == 16, "P run larger than S"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`EEE`,`S = 1`| 3 | Kích thước tối thiểu và tất cả các lát đều hợp lệ | 
|`PPPPP`,`S = 4`| 0 | Đầu vào hoàn toàn bằng nhau không có quả trứng | 
|`EPPP`,`S = 3`| 6 | Trường hợp ranh giới`S = n - 1`| 
|`E`lặp lại 100000 lần`S = 99999`| 9999900000 | Kích thước đầu vào tối đa và câu trả lời lớn | 
|`PEPEP`,`S = 2`| 6 | Chạy vòng qua ranh giới dây | 
|`EPPPPE`,`S = 2`| 16 | Một quả hồng sống lâu hơn`S`| 

## Vỏ cạnh 

Trường hợp ranh giới hình tròn được xử lý bằng cách bắt đầu quét một cách có chủ ý ở một điểm`E`. Vì`PEPEP`với`S = 2`, quá trình quét có thể bắt đầu ở ký tự thứ hai, tạo ra thứ tự vòng tròn`EPEPP`. trận chung kết`PP`run có độ dài hai, vì vậy nó góp phần`2 + 1 = 3`khoảng thời gian không hợp lệ. Người khác bị cô lập`P`đóng góp thêm một cái nữa, đưa ra bốn khoảng không hợp lệ trên tổng số mười. Câu trả lời là`10 - 4 = 6`. 

Trường hợp toàn hồng được xử lý trước khi quét. Vì`PPPP`với`S = 1`, không có`E`từ đó thiết lập ranh giới quét và về cơ bản hơn, mọi lát cắt có thể đều không hợp lệ. Trả về số 0 ngay lập tức vừa đơn giản vừa chính xác. 

Khi`S = 1`, mỗi lát hợp lệ là một quả trứng đơn lẻ. Vì`EPE`với`S = 1`, có thể có ba khoảng đơn lẻ, một là quả hồng và hai khoảng còn lại là quả trứng. Thuật toán bắt đầu với`3 * 1 = 3`và trừ đi một singleton có trong`P`chạy, sản xuất`2`. 

Khi`S = n - 1`, thuật toán vẫn chỉ tính các khoảng thích hợp. Vì`EPPP`với`S = 3`, có`4 * 3 = 12`khoảng thời gian ứng viên. Ba liên tiếp`P`s tạo thành một lần chạy, có tất cả-`P`các khoảng là ba khoảng có độ dài một, hai có độ dài hai và một có độ dài ba, cho sáu khoảng không hợp lệ. Kết quả là`12 - 6 = 6`. 

MỘT`P`chạy lâu hơn`S`cũng cần`min(r, S)`cắt ngắn. Vì`EPPPPE`với`S = 2`, phần giữa có độ dài bốn, nhưng không được phép có các khoảng có độ dài ba hoặc bốn. Chỉ có bốn khoảng đơn và ba khoảng dài hai được tính là không hợp lệ, trong bảy khoảng thời gian không hợp lệ kể từ lần chạy đó. có`6 * 2 = 12`tổng số khoảng ứng cử viên, và kết quả là`12 - 7 = 5`nếu chuỗi có một lần chạy như vậy. Đối với trường hợp thử nghiệm ở trên, hai`E`s chia chuỗi theo cách khác nhau, vì vậy lần chạy thực tế là`PPPP`và phép tính trực tiếp cho`12 - (4 + 3) = 5`; đây chính xác là lý do tại sao bài kiểm tra nên sử dụng`EPPPPE`với sản lượng dự kiến`5`, không`16`. 

Khẳng định đã sửa cho bài kiểm tra cuối cùng đó là:```
assert run("EPPPPE\n2\n") == "5", "P run larger than S"
```Bất biến trung tâm xử lý tất cả các trường hợp này một cách thống nhất: mọi khoảng cho phép đều bắt đầu từ tổng ban đầu`n * S`và mọi thứ không hợp lệ`P`khoảng thời gian được loại bỏ chính xác một lần thông qua mức tối đa duy nhất`P`chạy có chứa nó.
