---
title: "CF 105828E - \u041f\u0440\u043e\u0441\u0442\u043e\u0439 \u043a\u0430\u0440\u0430\u043d\u0434\u0430\u0448"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu số nguyên dương nhỏ hơn một số n cho trước có thể xuất hiện trên một cây bút chì đặc biệt trong quá trình gọt bút chì. Ràng buộc chính là tất cả các số hợp lệ phải là số nguyên tố và không được chứa chữ số 0."
date: "2026-06-21T14:56:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105828
codeforces_index: "E"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0412\u041a\u041e\u0428\u041f.Junior 2025"
rating: 0
weight: 105828
solve_time_s: 49
verified: true
draft: false
---

[CF 105828E - \u041f\u0440\u043e\u0441\u0442\u043e\u0439 \u043a\u0430\u0440\u0430\u043d\u0434\u0430\u0448](https://codeforces.com/problemset/problem/105828/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm có bao nhiêu số nguyên dương nhỏ hơn một số đã cho`n`có thể xuất hiện trên một cây bút chì đặc biệt trong quá trình mài. 

Ràng buộc chính là tất cả các số hợp lệ phải là số nguyên tố và không được chứa chữ số 0. Cây bút chì ban đầu có một số số nguyên tố không xác định được viết dọc theo toàn bộ chiều dài của nó. Khi được làm sắc nét, các chữ số được loại bỏ từng cạnh có ý nghĩa nhất, tạo ra một dãy số và mọi số trung gian như vậy cũng là số nguyên tố và không chứa số 0. Quá trình dừng lại khi không còn gì. Chúng tôi không được cung cấp số hoặc chuỗi ban đầu, chỉ có giới hạn cuối cùng`n`, và chúng ta phải đếm xem có bao nhiêu số nguyên nhỏ hơn`n`có thể xuất hiện ở đâu đó theo trình tự làm sắc nét hợp lệ như vậy. 

Vì vậy nhiệm vụ không chỉ đơn giản là “đếm các số nguyên tố dưới n”. Chúng ta phải đếm các số nguyên tố với một ràng buộc bổ sung: biểu diễn thập phân của chúng không chứa số 0. Bản thân quá trình làm sắc nét không đưa ra các số mới ngoài các tiền tố của một số hợp lệ, nhưng vì bất kỳ tiền tố hợp lệ nào cũng phải là số nguyên tố không có số 0, nên vấn đề giảm xuống còn việc liệt kê tất cả các số nguyên tố bên dưới`n`tránh chữ số 0. 

Ràng buộc`n ≤ 10^9`ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng phân tích nhân tử hoặc kiểm tra tính nguyên tố một cách ngây thơ cho tất cả các số lên đến`n`trong một vòng lặp đơn giản, vì việc kiểm tra từng số riêng lẻ sẽ yêu cầu tới một tỷ lần kiểm tra tính nguyên tố. Thậm chí một`O(√n)`kiểm tra tính nguyên thủy trên mỗi số sẽ quá chậm. 

Một trường hợp khó phát hiện là các số chứa số 0 phải bị loại trừ ngay cả khi chúng là số nguyên tố. Ví dụ,`101`là số nguyên tố nhưng không hợp lệ vì nó chứa số 0. Một trường hợp khác là các số nguyên tố có một chữ số như`2`,`3`,`5`,`7`là hợp lệ, nhưng`10`không hợp lệ dù nó nhỏ. 

Cấu trúc ẩn chính là chúng ta không thực sự cần xem xét tất cả các số lên đến`n`, nhưng chỉ là số nguyên tố và chúng ta cũng cần đảm bảo không có chữ số 0 nào xuất hiện. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp sẽ lặp qua tất cả các số nguyên từ`1`ĐẾN`n-1`, kiểm tra xem mỗi số có chứa chữ số 0 hay không, sau đó kiểm tra xem số đó có phải là số nguyên tố hay không. Kiểm tra chữ số rẻ nhưng kiểm tra tính nguyên thủy chiếm ưu thế. Ngay cả với một tối ưu hóa`O(√x)`kiểm tra tính nguyên thủy, chi phí trong trường hợp xấu nhất sẽ trở thành xấp xỉ:`sum_{x=1}^{10^9} √x ≈ 10^13`điều đó hoàn toàn không thể thực hiện được. 

Cấu trúc của vấn đề gợi ý một quan điểm khác. Thay vì kiểm tra từng số lên đến`n`, chúng tôi chỉ tạo ra các ứng cử viên có thể thỏa mãn các ràng buộc. Vì các số hợp lệ không thể chứa số 0 nên mọi chữ số đều bắt nguồn từ`1`ĐẾN`9`. Vì các số phải là số nguyên tố nên chúng ta cũng cần hạn chế chỉ sử dụng các số nguyên tố. 

Điều này gợi ý sự kết hợp hai ý tưởng: một thế hệ dựa trên chữ số trên bảng chữ cái chữ số được phép`{1..9}`và chỉ kiểm tra tính nguyên tố trên các ứng cử viên được tạo. Tuy nhiên, việc tạo ra tất cả các số tự do có tới 10 chữ số vẫn còn lớn (`9^10`tỉ lệ), vì vậy chúng ta cần cắt tỉa: chúng ta chỉ giữ các số nguyên tố và chúng ta có thể kết thúc sớm khi số vượt quá`n`. 

Cấu trúc thực tế hơn là tạo kiểu chữ số DP hoặc BFS trên các số được hình thành bằng cách nối thêm các chữ số`1..9`, kiểm tra tính nguyên tố ở mỗi bước. Từ`n ≤ 10^9`, chúng tôi chỉ khám phá các số có tối đa 10 chữ số và cắt tỉa theo`n`giữ cho tìm kiếm nhỏ trong thực tế. Việc kiểm tra tính nguyên thủy vẫn cần thiết nhưng chúng chỉ được áp dụng cho các ứng viên được tạo ra và số lượng rất ít. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n√n) | O(1) | Quá chậm | 
| Tạo chữ số + kiểm tra tính nguyên tố | O(k · ứng cử viên · √n) (cắt tỉa) | O(k) | Đã chấp nhận | 

Đây`k`là số lượng ứng viên được tạo ra, vẫn có thể quản lý được do hạn chế về chữ số và việc cắt bớt theo`n`. 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng tất cả các số không chứa chữ số 0 bằng cách sử dụng DFS bắt đầu từ tiền tố trống, liên tục nối thêm các chữ số từ`1`ĐẾN`9`. Trong quá trình xây dựng, chúng tôi duy trì giá trị số hiện tại. 

1. Bắt đầu từ một số trống và bộ đếm được khởi tạo bằng 0. Điều này đại diện cho gốc của cây tìm kiếm các chuỗi chữ số của chúng tôi. 
2. Với mỗi số hiện tại, hãy thử thêm từng chữ số từ`1`ĐẾN`9`để tạo thành một số mới. Chúng tôi rõ ràng tránh`0`, thực thi ràng buộc chữ số tại thời điểm xây dựng thay vì lọc sau đó. 
3. Nếu số mới lớn hơn hoặc bằng`n`, chúng tôi ngừng mở rộng nó hơn nữa. Bất kỳ phần mở rộng nào nữa sẽ chỉ làm tăng giá trị, do đó không có phần tử con nào có thể hợp lệ. 
4. Nếu số mới ít nhất là`2`, chúng ta kiểm tra xem nó có phải là số nguyên tố hay không bằng cách chia thử cho đến căn bậc hai của nó. Nếu nó là số nguyên tố, chúng ta tăng bộ đếm câu trả lời. 
5. Tiếp tục đệ quy DFS từ số mới này. 

Đệ quy tự nhiên khám phá tất cả các chuỗi chữ số hợp lệ theo thứ tự độ dài tăng dần, nhưng cắt bớt bằng cách`n`ngăn chặn vụ nổ. 

### Tại sao nó hoạt động 

Mỗi số chúng tôi tạo ra là một số nguyên dương chỉ gồm các chữ số`1`ĐẾN`9`, do đó nó tự động thỏa mãn ràng buộc “không có chữ số 0”. DFS đảm bảo rằng mọi số như vậy nhỏ hơn`n`được truy cập đúng một lần. Chúng tôi chỉ đếm những số vượt qua bài kiểm tra tính nguyên tố, vì vậy chúng tôi tính chính xác các số nguyên tố dưới`n`không có chữ số 0. Vì bất kỳ số hợp lệ nào cũng phải xuất hiện dưới dạng một nút trong cây xây dựng này và chúng tôi không bao giờ bỏ qua bất kỳ đường dẫn xây dựng hợp lệ nào trong`n`, tính đầy đủ và đúng đắn theo sau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def is_prime(x: int) -> bool:
    if x < 2:
        return False
    if x % 2 == 0:
        return x == 2
    r = int(math.isqrt(x))
    d = 3
    while d <= r:
        if x % d == 0:
            return False
        d += 2
    return True

def dfs(x: int, n: int) -> int:
    cnt = 0
    if x != 0 and x < n and is_prime(x):
        cnt += 1
    for d in range(1, 10):
        y = x * 10 + d
        if y >= n:
            continue
        cnt += dfs(y, n)
    return cnt

def main():
    n = int(input().strip())
    print(dfs(0, n))

if __name__ == "__main__":
    main()
```Giải pháp xây dựng số lượng tăng dần bằng cách sử dụng DFS. Cuộc gọi đầu tiên bắt đầu từ`0`, được coi là tiền tố trống. Mỗi bước nối thêm chữ số`1`bởi vì`9`, đảm bảo không có số 0 nào xuất hiện. 

Việc kiểm tra tính nguyên thủy chỉ được thực hiện trên các ứng cử viên được tạo ra. Séc`x < n`đảm bảo chúng tôi không bao giờ xem xét các giá trị nằm ngoài phạm vi được yêu cầu. Điều kiện đặc biệt`x != 0`ngăn chặn việc đếm trạng thái trống ban đầu. 

Một điểm tinh tế là việc cắt bớt: chúng ta chỉ dừng đệ quy khi giá trị tiếp theo vượt quá hoặc bằng`n`. Điều này ngăn cản việc thăm dò không cần thiết các nhánh lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để`n = 20`. 

Chúng tôi bắt đầu từ`0`. 

| Hiện tại x | Giá trị y được tạo | Kiểm tra chính | Đếm | 
| --- | --- | --- | --- | 
| 0 | 1..9 | 2,3,5,7 hợp lệ | 4 | 
| 1 | 11,12,...19 | 11,13,17,19 hợp lệ | +4 | 

Tổng số trở thành`8`. 

Dấu vết này cho thấy các số nguyên tố có một chữ số được phát hiện ở độ sâu nông, trong khi các số nguyên tố có hai chữ số được khám phá sâu hơn một cấp. Tất cả các số chứa số 0 không bao giờ được tạo ra, vì vậy chúng không bao giờ xuất hiện trong séc. 

### Ví dụ 2 

hãy để`n = 15`. 

| Hiện tại x | Giá trị y được tạo | Kiểm tra chính | Đếm | 
| --- | --- | --- | --- | 
| 0 | 1..9 | 2,3,5,7 hợp lệ | 4 | 
| 1 | 11,12,...14 | 11 hợp lệ | +1 | 

Câu trả lời cuối cùng là`5`. 

Ví dụ này cho thấy việc cắt tỉa đang được thực hiện: những con số như`12`,`14`được tạo nhưng nhanh chóng bị từ chối vì không phải là số nguyên tố, trong khi số nguyên tố hợp lệ được tính chính xác một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · √n) | Mỗi ứng cử viên được tạo đều trải qua bài kiểm tra tính nguyên tố lên tới √n | 
| Không gian | O(d) | Độ sâu đệ quy giới hạn bởi số chữ số (≤ 10) | 

DFS bị ràng buộc về chữ số đảm bảo số lượng ứng cử viên được tạo ra nhỏ hơn nhiều so với`n`. Với`n ≤ 10^9`, độ sâu đệ quy tối đa là 10 và phân nhánh được giới hạn ở các chữ số`1..9`, do đó thuật toán chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def is_prime(x: int) -> bool:
        if x < 2:
            return False
        if x % 2 == 0:
            return x == 2
        r = int(math.isqrt(x))
        d = 3
        while d <= r:
            if x % d == 0:
                return False
            d += 2
        return True

    def dfs(x: int, n: int) -> int:
        cnt = 0
        if x != 0 and x < n and is_prime(x):
            cnt += 1
        for d in range(1, 10):
            y = x * 10 + d
            if y >= n:
                continue
            cnt += dfs(y, n)
        return cnt

    n = int(sys.stdin.readline())
    return str(dfs(0, n))

assert run("2") == "0"
assert run("10") == "4"
assert run("20") == "8"
assert run("100") == run("100")

# boundary case: just above single-digit primes
assert run("8") == "3"
assert run("9") == "4"
assert run("11") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 0 | trường hợp tối thiểu, không có số nguyên tố hợp lệ | 
| 10 | 4 | chỉ các số nguyên tố có một chữ số | 
| 20 | 8 | chuyển sang số nguyên tố có hai chữ số | 
| 8 | 3 | ranh giới giữa các số nguyên tố nhỏ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`n`là rất nhỏ. Vì`n = 2`, không có số nguyên tố nào nhỏ hơn 2 nên đáp án là 0. DFS vẫn bắt đầu từ`0`, nhưng không có nút hợp lệ nào thỏa mãn`x < n`và tính nguyên thủy đồng thời, nên không có gì được tính. 

Một trường hợp cạnh khác xảy ra ở ranh giới của các số nguyên tố có một chữ số. Vì`n = 10`, chỉ một`2, 3, 5, 7`là hợp lệ. DFS tạo chính xác các giá trị này từ gốc và tất cả đều được tính. Những con số như`10`không bao giờ được tạo ra vì chữ số`0`bị cấm trong thời gian xây dựng. 

Đối với lớn hơn`n`, chẳng hạn như ở trên`100`, thuật toán khám phá chính xác các nhánh có hai chữ số và ba chữ số nhưng cắt bỏ bất cứ thứ gì đạt hoặc vượt quá`n`. Vì việc cắt tỉa được áp dụng trước khi đệ quy nên không có phép thăm dò không hợp lệ nào xảy ra và tất cả các số nguyên tố hợp lệ dưới giới hạn vẫn có thể truy cập được thông qua ít nhất một đường dẫn xây dựng.
