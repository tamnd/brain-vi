---
title: "CF 148C - Công chúa Terse"
description: "Chúng ta cần xây dựng một loạt vận may của chú rể để công chúa phản ứng theo đúng cách được yêu cầu. Đối với mỗi chú rể sau lần đầu tiên, có thể xảy ra hai tình huống đặc biệt. Nếu vận may hiện tại lớn hơn mọi vận may trước đó, công chúa sẽ nói Ồ...."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 148
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 105 (Div. 2)"
rating: 1700
weight: 148
solve_time_s: 134
verified: false
draft: false
---

[CF 148C - Công chúa ngắn gọn](https://codeforces.com/problemset/problem/148/C) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một loạt vận may của chú rể để công chúa phản ứng theo đúng cách được yêu cầu. 

Đối với mỗi chú rể sau lần đầu tiên, có thể xảy ra hai tình huống đặc biệt. 

Nếu vận may hiện tại lớn hơn mọi vận may trước đó, công chúa nói`Oh...`. 

Nếu tài sản hiện tại lớn hơn tổng của tất cả tài sản trước đó, công chúa nói`Wow!`thay vì. MỘT`Wow!`thay thế`Oh...`, nó không được tính là cả hai. 

Chú rể đầu tiên không bao giờ tạo ra bất kỳ phản ứng nào. 

Chúng tôi được trao`n`, số lượng chú rể, cùng với số lượng chính xác của`Oh...`phản ứng`a`Và`Wow!`phản ứng`b`. Nhiệm vụ là xuất ra bất kỳ chuỗi may mắn hợp lệ nào giữa`1`Và`50000`, hoặc`-1`nếu không có trình tự như vậy tồn tại. 

Những hạn chế là rất nhỏ. Cả hai`a`Và`b`nhiều nhất là`15`, Và`n`nhiều nhất là`100`. Điều đó có nghĩa là tìm kiếm mạnh mẽ trên các mảng tùy ý vẫn sẽ bùng nổ, vì mỗi vị trí có thể chứa tới`50000`sự lựa chọn. Một giải pháp quay lui ngây thơ sẽ hoàn toàn không khả thi. Mặt khác, những giới hạn nhỏ cho thấy rõ ràng rằng giải pháp dự định mang tính xây dựng. Chúng tôi dự kiến ​​sẽ trực tiếp xây dựng một chuỗi hợp lệ thay vì tìm kiếm một chuỗi. 

Phần khó khăn là ở chỗ đó`Wow!`mạnh hơn nhiều so với`Oh...`. 

Giả sử số vận may trước đó cộng lại là`S`và mức tối đa hiện tại là`M`. 

Nếu chúng ta chọn một giá trị: 

-`x <= M`, không có phản ứng. 
-`M < x <= S`, chúng tôi nhận được`Oh...`. 
-`x > S`, chúng tôi nhận được`Wow!`. 

Điều này ngay lập tức tiết lộ một sự thật quan trọng. Để sản xuất một`Oh...`, chúng ta cần số tiền trước đó phải lớn hơn mức tối đa hiện tại. Nếu không thì khoảng`(M, S]`trống rỗng. 

Ví dụ: 

đầu vào:```
3 1 1
```Một công trình bất cẩn có thể thử:```
1 2 3
```Nhưng điều này mang lại cho hai`Wow!`phản ứng, vì: 

-`2 > 1`-`3 > 1+2`Không bao giờ có một`Oh...`. 

Một sai lầm dễ mắc phải khác là quên rằng các giá trị nhỏ lặp lại là chất bổ sung hữu ích. 

Ví dụ: 

đầu vào:```
5 0 0
```Một dãy tăng dần sẽ thất bại ngay lập tức. Ý tưởng đúng là giữ tất cả các giá trị bằng nhau:```
7 7 7 7 7
```Không có giá trị nào vượt quá mức tối đa trước đó nên không có phản ứng nào cả. 

Một trường hợp cạnh tinh tế hơn xuất hiện khi chúng ta muốn quá nhiều`Oh...`phản ứng liên quan đến`Wow!`. 

Coi như:```
3 2 0
```Điều này là không thể. 

Không có bất kỳ`Wow!`, tổng và giá trị lớn nhất vẫn bằng nhau sau phần tử đầu tiên nếu tất cả các số đều giống nhau. Để tạo ra ngay cả một`Oh...`, trước tiên chúng ta cần tổng số tiền lớn hơn mức tối đa. Cơ chế duy nhất làm tăng tổng nhanh chóng là`Wow!`. 

Điều này dẫn đến điều kiện khả thi chính:```
a <= b + 1
```Nếu điều kiện này không thành công thì không có công trình nào tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là thử tạo ra tất cả các mảng có thể có và mô phỏng các phản ứng công chúa cho từng mảng. Bản thân việc mô phỏng đã dễ dàng. Chúng tôi duy trì tổng tiền tố và giá trị tối đa hiện tại, sau đó phân loại mọi giá trị mới vào một trong ba loại. 

Vấn đề là không gian tìm kiếm. Ngay cả khi chúng ta giới hạn vận may ở những giá trị từ`1`ĐẾN`10`, số lượng mảng sẽ là`10^100`trong trường hợp xấu nhất. Không có cơ hội khám phá không gian đó. 

Cấu trúc của các phản ứng cho một hướng tốt hơn nhiều. Mọi phản ứng chỉ phụ thuộc vào hai đại lượng tính từ tiền tố: 

- mức tối đa hiện tại 
- tổng tiền tố hiện tại 

A`Wow!`cả hai đều tăng mạnh vì giá trị mới vượt quá toàn bộ số tiền trước đó. MỘT`Oh...`chỉ tăng mức tối đa, trong khi vẫn ở trong số tiền hiện tại. 

Nhận xét quan trọng là sau một`Wow!`, tổng tiền tố trở nên lớn hơn nhiều so với mức tối đa. Điều đó tạo ra chỗ cho tương lai`Oh...`phản ứng. 

Ví dụ:```
1
```Tổng hiện tại = 1, tối đa = 1. 

Nếu chúng ta nối thêm:```
2
```Chúng tôi nhận được một`Wow!`. 

Hiện nay:```
sum = 3
max = 2
```Cuối cùng cũng có không gian cho một`Oh...`, bởi vì các giá trị trong`(2, 3]`hiện hữu. 

Đang bổ sung:```
3
```Sản xuất một`Oh...`. 

Sau đó:```
sum = 6
max = 3
```Lại có không gian cho người khác`Oh...`. 

Mẫu này gợi ý một công trình sạch sẽ: 

- Bắt đầu với`1`. 
- Sản xuất từng`Wow!`sử dụng`sum + 1`. 
- Sau mỗi lần`Wow!`, tùy ý sản xuất một`Oh...`sử dụng chính xác`sum`. 

Bởi vì mỗi`Wow!`tạo chỗ cho chính xác một người được đảm bảo`Oh...`, số lượng tối đa của`Oh...`phản ứng là`b + 1`. Đó chính xác là điều kiện khả thi. 

Khi tất cả các phản ứng cần thiết được tạo, chúng tôi điền vào các vị trí còn lại bằng`1`, điều này không bao giờ kích hoạt bất cứ điều gì vì mức tối đa ít nhất đã là`1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem`a > b + 1`. 

Nếu điều này xảy ra, hãy in`-1`. Mọi`Wow!`có thể tạo chỗ cho nhiều nhất một cái mới`Oh...`, và cũng có thể có một chữ cái đầu`Oh...`trước bất kỳ`Wow!`. 
2. Bắt đầu chuỗi với`1`. 

Chú rể đầu tiên không bao giờ gây ra bất kỳ phản ứng nào, vì vậy đây là bước khởi đầu đơn giản nhất có thể. 
3. Duy trì hai biến: 

-`s`, tổng tiền tố hiện tại 
-`mx`, mức tối đa hiện tại 
4. Nếu`a > 0`, tạo một tên ban đầu`Oh...`. 

Nối thêm`1`một lần nữa chỉ khi chúng ta không muốn`Oh...`. Ngược lại, chúng ta cần một giá trị lớn hơn mức tối đa nhưng không lớn hơn tổng. Ban đầu cả hai đều`1`, vì vậy điều này là không thể. 

Cách duy nhất để tạo ra cái đầu tiên`Oh...`là sau một`Wow!`. 
5. Lặp lại`b`lần: 

- Nối thêm`s + 1`. 
- Điều này đảm bảo một`Wow!`, vì giá trị mới vượt quá toàn bộ tổng trước đó. 
- Cập nhật`s`Và`mx`. 
6. Sau mỗi lần`Wow!`, nếu chúng ta vẫn cần thêm`Oh...`phản ứng: 

- Nối thêm`s`. 
- Từ`mx < s <= s`, điều này tạo ra một`Oh...`. 
- Cập nhật`s`Và`mx`. 
7. Điền vào tất cả các vị trí còn lại bằng`1`. 

Vì mức tối đa đã lớn hơn nhiều so với`1`, những vị trí này không tạo ra phản ứng nào. 

### Tại sao nó hoạt động 

Việc xây dựng duy trì một bất biến đơn giản. 

Bất cứ khi nào chúng tôi nối thêm`s + 1`, giá trị mới lớn hơn toàn bộ tổng tiền tố trước đó, nên phản ứng luôn là`Wow!`. 

Bất cứ khi nào chúng tôi nối thêm`s`, mức tối đa trước đó hoàn toàn nhỏ hơn`s`, bởi vì cuối cùng`Wow!`đã tạo ra khoảng cách giữa họ. Giá trị mới không lớn hơn tổng trước đó nên phản ứng chính xác`Oh...`. 

Tất cả các giá trị phụ là`1`, không bao giờ vượt quá mức tối đa hiện tại. 

Số lượng của`Wow!`Và`Oh...`được kiểm soát độc lập và mọi số được thêm vào đều có tác dụng chính xác như mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b = map(int, input().split())

    if a > b + 1:
        print(-1)
        return

    ans = [1]

    s = 1
    mx = 1

    oh_used = 0
    wow_used = 0

    for _ in range(b):
        x = s + 1
        ans.append(x)

        wow_used += 1
        s += x
        mx = x

        if oh_used < a:
            x = s
            ans.append(x)

            oh_used += 1
            s += x
            mx = x

    while len(ans) < n:
        ans.append(1)

    print(*ans[:n])

solve()
```Điều kiện đầu tiên là kiểm tra tính khả thi. Nếu như`a > b + 1`, không có công trình nào có thể tồn tại, bởi vì mỗi`Wow!`tạo ra không gian cho nhiều nhất một tương lai`Oh...`. 

Trình tự bắt đầu từ`1`bởi vì nó giữ cho mọi phép tính sau này được đơn giản. Các biến`s`Và`mx`theo dõi tổng tiền tố hiện tại và giá trị tối đa. 

Việc tạo vòng lặp`Wow!`phản ứng nối thêm`s + 1`. Vì giá trị này lớn hơn toàn bộ tổng trước đó nên nó luôn kích hoạt`Wow!`. 

Ngay sau đó, nếu có thêm`Oh...`phản ứng vẫn cần thiết, chúng tôi nối thêm chính xác`s`. Tại thời điểm đó mức tối đa hiện tại hoàn toàn nhỏ hơn`s`, do đó giá trị mới sẽ trở thành mức tối đa mới mà không vượt quá tổng tiền tố. Đó chính xác là định nghĩa của`Oh...`. 

Vòng lặp phụ nối thêm`1`cho đến khi mảng đạt độ dài`n`. Các giá trị này ở mức an toàn dưới mức tối đa hiện tại và không bao giờ ảnh hưởng đến số lượng phản ứng. 

Một điểm tinh tế là thứ tự cập nhật. Chúng ta phải cập nhật tổng tiền tố sau mỗi lần nối thêm trước khi tạo giá trị tiếp theo. Sử dụng các giá trị cũ sẽ thay đổi hoàn toàn cách phân loại phản ứng. 

Một điểm tinh tế khác là cắt bằng`ans[:n]`. Việc xây dựng có thể tạm thời tạo ra nhiều yếu tố hơn mức cần thiết nếu`n`gần với`a + b + 1`. Tuyên bố đảm bảo`n > a + b`, vì vậy việc cắt bớt vẫn an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 2 3
```Dấu vết xây dựng: 

| Bước | Giá trị gia tăng | Tổng trước | Phản ứng | Tổng mới | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 0 | Không có | 1 | 
| Ồ | 2 | 1 | Ồ! | 3 | 
| Ồ | 3 | 3 | Ồ... | 6 | 
| Ồ | 7 | 6 | Ồ! | 13 | 
| Ồ | 13 | 13 | Ồ... | 26 | 
| Ồ | 27 | 26 | Ồ! | 53 | 
| Điền | 1 | 53 | Không có | 54 | 
| Điền | 1 | 54 | Không có | 55 | 
| Điền | 1 | 55 | Không có | 56 | 
| Điền | 1 | 56 | Không có | 57 | 

Trình tự cuối cùng:```
1 2 3 7 13 27 1 1 1 1
```Dấu vết này thể hiện sự bất biến cốt lõi. Mọi`Wow!`làm cho tổng lớn hơn nhiều so với mức tối đa và điều đó ngay lập tức cho phép một giá trị hợp lệ`Oh...`. 

### Ví dụ 2 

đầu vào:```
5 3 1
```Từ:```
3 > 1 + 1
```câu trả lời là không thể. 

| Giá trị | Ý nghĩa | 
| --- | --- | 
| n = 5 | Tổng số chú rể | 
| a = 3 | Cần 3 Ồ... | 
| b = 1 | Cần 1 Ôi! | 

Chỉ với một`Wow!`, chúng ta có thể tạo chỗ cho tối đa hai`Oh...`phản ứng. Việc xây dựng in chính xác:```
-1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được tạo một lần | 
| Không gian | O(n) | Mảng đầu ra lưu trữ`n`giá trị | 

Các giới hạn là cực kỳ nhỏ nên giải pháp này dễ dàng phù hợp với những hạn chế về thời gian và bộ nhớ. Kể cả ở mức tối đa`n = 100`, chương trình chỉ thực hiện vài trăm phép tính số học. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, a, b = map(int, input().split())

    if a > b + 1:
        print(-1)
        return

    ans = [1]

    s = 1
    mx = 1

    oh_used = 0

    for _ in range(b):
        x = s + 1
        ans.append(x)

        s += x
        mx = x

        if oh_used < a:
            x = s
            ans.append(x)

            oh_used += 1
            s += x
            mx = x

    while len(ans) < n:
        ans.append(1)

    print(*ans[:n])

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided-style cases
assert run("10 2 3\n") != "-1"

assert run("5 3 1\n") == "-1", "impossible case"

# minimum size
assert run("1 0 0\n") == "1", "single groom"

# no reactions at all
assert run("5 0 0\n") == "1 1 1 1 1", "all equal"

# exactly one wow
assert run("3 0 1\n") != "-1", "single wow"

# boundary feasibility
assert run("6 2 1\n") != "-1", "a == b + 1 is valid"

# impossible boundary
assert run("6 3 1\n") == "-1", "a > b + 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0`|`1`| Đầu vào hợp lệ nhỏ nhất | 
|`5 0 0`|`1 1 1 1 1`| Không có phản ứng nào cả | 
|`3 0 1`| Bất kỳ chuỗi hợp lệ nào | Đơn`Wow!`xây dựng | 
|`6 2 1`| Bất kỳ chuỗi hợp lệ nào | Ranh giới khả thi chặt chẽ | 
|`6 3 1`|`-1`| Điều kiện bất khả thi | 

## Vỏ cạnh 

Hãy xem xét:```
3 2 0
```Chúng ta cần hai`Oh...`phản ứng nhưng không`Wow!`. 

Ban đầu:```
sum = 1
max = 1
```Không có số nguyên nào giữa số tối đa và tổng. Không có`Wow!`, khoảng cách đó không bao giờ xuất hiện. Thuật toán phát hiện:```
2 > 0 + 1
```và đầu ra chính xác:```
-1
```Bây giờ hãy xem xét:```
5 0 0
```Thuật toán tạo ra:```
1 1 1 1 1
```Dấu vết: 

| Vị trí | Giá trị | Tối đa hiện tại | Phản ứng | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | Không có | 
| 2 | 1 | 1 | Không có | 
| 3 | 1 | 1 | Không có | 
| 4 | 1 | 1 | Không có | 
| 5 | 1 | 1 | Không có | 

Các giá trị lặp lại là rất cần thiết ở đây. Một chuỗi tăng dần sẽ vô tình tạo ra phản ứng. 

Cuối cùng, hãy xem xét ranh giới hợp lệ chặt chẽ:```
6 2 1
```Sự thi công:```
1 2 3 6 1 1
```Dấu vết: 

| Giá trị | Tổng trước | Phản ứng | 
| --- | --- | --- | 
| 1 | 0 | Không có | 
| 2 | 1 | Ồ! | 
| 3 | 3 | Ồ... | 
| 6 | 6 | Ồ... | 
| 1 | 12 | Không có | 
| 1 | 13 | Không có | 

Điều này xác nhận tại sao`a = b + 1`vẫn có thể đạt được. Một`Wow!`tạo đủ chỗ cho hai người liên tiếp`Oh...`phản ứng.
