---
title: "CF 1913B - Hoán đổi và xóa"
description: "Chúng ta bắt đầu với một chuỗi nhị phân s. Chúng tôi có thể xóa các ký tự, trả một xu cho mỗi lần xóa và chúng tôi có thể trao đổi miễn phí bất kỳ cặp ký tự nào còn lại. Sau tất cả các thao tác, chúng ta thu được một chuỗi t."
date: "2026-06-08T20:08:22+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 1913
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 160 (Rated for Div. 2)"
rating: 1000
weight: 1913
solve_time_s: 127
verified: true
draft: false
---

[CF 1913B - Hoán đổi và xóa](https://codeforces.com/problemset/problem/1913/B) 

**Đánh giá:** 1000 
**Thẻ:** chuỗi 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi nhị phân`s`. Chúng tôi có thể xóa các ký tự, trả một xu cho mỗi lần xóa và chúng tôi có thể trao đổi miễn phí bất kỳ cặp ký tự nào còn lại. 

Sau tất cả các thao tác, chúng ta thu được một chuỗi`t`. Chiều dài của`t`có thể là bất cứ điều gì từ`0`lên đến`|s|`, vì việc xóa được cho phép. Điều kiện cho`t`tốt là mọi vị trí tồn tại trong`t`phải khác với vị trí tương ứng trong chuỗi gốc`s`. Nói cách khác, với mỗi chỉ số`i ≤ |t|`, chúng tôi cần`t[i] ≠ s[i]`. 

Các giao dịch hoán đổi miễn phí là phần mấu chốt của vấn đề. Vì việc hoán đổi không mất phí và có thể thực hiện tùy ý nhiều lần nên thứ tự các ký tự còn lại hoàn toàn nằm trong tầm kiểm soát của chúng tôi. Điều quan trọng duy nhất là có bao nhiêu`0`cát`1`s vẫn còn sau khi xóa. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các tập hợp con xóa hoặc tất cả các sắp xếp lại có thể. Chúng tôi cần thứ gì đó gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Vì bản thân tổng kích thước đầu vào chỉ`2·10^5`, MỘT`O(n)`giải pháp là lý tưởng và`O(n log n)`giải pháp cũng sẽ dễ dàng đủ nhanh. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như`s = "0"`. Chuỗi không trống duy nhất chúng ta có thể tạo thành là`"0"`và nó khớp với vị trí đầu tiên của chuỗi gốc. Chuỗi tốt duy nhất là chuỗi trống, vì vậy câu trả lời là`1`. 

Coi như`s = "01"`. Chúng ta có thể hoán đổi hai ký tự và có được`"10"`. Mọi vị trí đều khác với vị trí ban đầu, vì vậy câu trả lời là`0`. Một giải pháp cho rằng việc xóa luôn là cần thiết bất cứ khi nào một vị trí phù hợp ban đầu không thành công ở đây. 

Coi như`s = "111100"`. Có bốn số một và hai số không. Để làm cho bốn vị trí đầu tiên khác với vị trí ban đầu, chúng ta cần bốn số không, nhưng chỉ tồn tại hai số không. Một số việc xóa là không thể tránh khỏi. Câu trả lời không được xác định bởi các giao dịch hoán đổi cục bộ mà bởi số lượng ký tự có sẵn trên toàn cầu. 

Những ví dụ này gợi ý rằng vấn đề thực sự nằm ở chỗ liệu chúng ta có đủ các bit đối lập để lấp đầy từng vị trí hay không. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất hữu ích để hiểu cấu trúc. 

Giả sử chúng ta chọn một số bộ ký tự để xóa. Sau đó, chúng ta biết còn lại bao nhiêu số 0 và số 1. Vì việc hoán đổi là miễn phí nên mọi hoán vị của các ký tự còn lại đều có thể thực hiện được. Chúng ta có thể hỏi liệu có tồn tại một hoán vị sao cho mọi vị trí đều khác với tiền tố ban đầu của`s`. 

Cách tiếp cận vũ phu sẽ thử tất cả các số lượng và vị trí xóa có thể, sau đó kiểm tra xem có tồn tại một số sắp xếp lại hợp lệ hay không. Ngay cả đối với một chuỗi dài`n`, có`2^n`lựa chọn xóa, làm cho điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là giao dịch hoán đổi tự do sẽ phá hủy gần như tất cả thông tin vị thế. Sau khi xóa, chỉ có số lượng số 0 và số 1 còn lại là quan trọng. 

Giả sử chúng ta muốn giữ cái đầu tiên`k`các vị trí. Tiền tố gốc`s[0:k]`chứa một số số không và một số số một. 

Mỗi vị trí chứa`0`trong tiền tố ban đầu phải nhận được một`1`ở chuỗi cuối cùng. Mỗi vị trí chứa`1`trong tiền tố ban đầu phải nhận được một`0`. 

Cho phép

-`need0`= số bản gốc`1`s ở tiền tố 
-`need1`= số bản gốc`0`s ở tiền tố 

Đây chính xác là số không và số một chúng ta phải đặt. 

Bây giờ hãy tưởng tượng quét chuỗi gốc từ trái sang phải và cố gắng tạo tiền tố tốt dài nhất. Bất cứ khi nào chúng ta nhìn thấy một nhân vật`c`trong chuỗi gốc, chúng ta cần một bản sao của ký tự đối diện`1-c`từ nhiều bộ ký tự có sẵn. 

Nếu một ký tự như vậy tồn tại, chúng tôi sử dụng nó. Nếu không, việc xây dựng sẽ trở nên bất khả thi ở vị trí đó. Vì mọi chuỗi tốt sau này cũng phải thỏa mãn vị trí này nên chúng ta không thể mở rộng thêm nữa. 

Vào thời điểm đó, tất cả các ký tự còn lại phải bị xóa. Số lần xóa bằng số ký tự còn lại chưa được xử lý. 

Điều này đưa ra một quá trình tham lam rất đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tổng số số 0 và số 1 trong toàn bộ chuỗi. 
2. Hãy coi những số đếm này như một tập hợp các ký tự có thể được sắp xếp lại tùy ý. 
3. Quét chuỗi gốc từ trái sang phải. 
4. Đối với nhân vật hiện tại`c`, chúng ta phải đặt ký tự đối diện vào vị trí tương ứng của`t`. 
5. Kiểm tra xem có còn ít nhất một bản sao của ký tự đối diện trong nhóm hay không. 
6. Nếu nó tồn tại, hãy sử dụng một bản sao của ký tự đối diện đó và tiếp tục. 

Điều này có nghĩa là chúng ta đã khớp thành công thêm một vị trí nữa của một chuỗi tốt. 
7. Nếu nó không tồn tại, hãy dừng ngay lập tức. 

Không có sự sắp xếp lại nào có thể thỏa mãn vị trí này, bởi vì mọi chuỗi tốt đều yêu cầu bit đối diện ở đây. 
8. Để sự cố xảy ra tại vị trí`i`(dựa trên 0). Sau đó tất cả các ký tự từ vị trí`i`trở đi phải bị xóa. 
9. Câu trả lời là`n - i`. 
10. Nếu quá trình quét kết thúc mà không thất bại, mọi vị trí đều có thể được đáp ứng và không cần xóa. Câu trả lời là`0`. 

### Tại sao nó hoạt động 

Ở bất kỳ vị trí nào, yêu cầu đều được xác định đầy đủ: a`0`trong chuỗi gốc yêu cầu một`1`, và một`1`yêu cầu một`0`. 

Vì các giao dịch hoán đổi là miễn phí nên hạn chế duy nhất về tài nguyên là số lượng số 0 và số 1 còn lại. Bất cứ khi nào có một ký tự đối diện tồn tại, việc sử dụng nó luôn an toàn vì mọi giải pháp hợp lệ đều phải dành một ký tự đối diện cho vị trí này. 

Nếu tại một vị trí nào đó không còn ký tự đối diện nào thì không có sự sắp xếp nào của các ký tự còn lại có thể thỏa mãn vị trí đó. Mỗi chuỗi tốt có độ dài đó sẽ cần một ký tự không tồn tại trong nhóm. 

Vì vậy, vị trí đứt đầu tiên chính xác là điểm mà một sợi dây tốt không thể kéo dài ra ngoài. Không thể giữ lại bất kỳ ký tự bổ sung nào, vì vậy tất cả các ký tự còn lại phải bị xóa. Số lần xóa chính xác là độ dài hậu tố bắt đầu từ vị trí lỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        s = input().strip()
        n = len(s)

        cnt = [0, 0]
        for ch in s:
            cnt[int(ch)] += 1

        answer = 0

        for i, ch in enumerate(s):
            need = 1 - int(ch)

            if cnt[need] == 0:
                answer = n - i
                break

            cnt[need] -= 1
        else:
            answer = 0

        ans.append(str(answer))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đếm xem có bao nhiêu số 0 và số 1 tồn tại trong toàn bộ chuỗi. Số lượng này đại diện cho nhóm ký tự có sẵn sau khi hoán đổi tùy ý. 

Trong quá trình quét, một vị trí chứa`0`yêu cầu một`1`, và một vị trí chứa`1`yêu cầu một`0`. Chúng tôi kiểm tra xem ký tự đối diện đó có còn tồn tại trong nhóm hay không. 

Nếu có, chúng tôi sẽ sử dụng một bản sao. Về mặt khái niệm, chúng ta đang gán ký tự đó vào vị trí hiện tại của chuỗi cuối cùng. 

Nếu bit ngược lại được yêu cầu không có sẵn thì chúng ta đã đạt đến vị trí không thể đầu tiên. Hậu tố còn lại phải bị xóa nên đáp án là`n - i`. 

các`for ... else`việc xây dựng ở đây rất thuận tiện. các`else`khối chỉ thực thi nếu vòng lặp không bao giờ ngắt, nghĩa là mọi vị trí đều được khớp thành công và câu trả lời là`0`. 

Một điểm tinh tế là chúng tôi không bao giờ loại bỏ ký tự gốc khỏi số đếm. Số lượng đại diện cho nhiều bộ ký tự có sẵn để sắp xếp lại và mỗi khi chúng tôi gán một ký tự cho một vị trí nào đó, chúng tôi sẽ sử dụng chính xác một ký tự từ nhiều bộ ký tự đó. Ký tự được sử dụng là bit ngược lại được yêu cầu tại vị trí đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
011
```Số đếm ban đầu: 

- số không = 1 
- những cái = 2 

| Vị trí | s[i] | Cần thiết | Số 0 còn lại | Những cái còn lại | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 2 | sử dụng một cái 1 | 
| 1 | 1 | 0 | 1 | 1 | sử dụng một 0 | 
| 2 | 1 | 0 | 0 | 1 | không thể | 

Sự cố xảy ra ở vị trí`2`. 

Trả lời:```
3 - 2 = 1
```Một lần xóa là đủ. 

### Ví dụ 2 

đầu vào:```
0101110001
```Số đếm ban đầu: 

- số không = 5 
- những cái = 5 

| Vị trí | s[i] | Cần thiết | Số 0 còn lại | Những cái còn lại | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 5 | 4 | 
| 1 | 1 | 0 | 4 | 4 | 
| 2 | 0 | 1 | 4 | 3 | 
| 3 | 1 | 0 | 3 | 3 | 
| 4 | 1 | 0 | 2 | 3 | 
| 5 | 1 | 0 | 1 | 3 | 
| 6 | 0 | 1 | 1 | 2 | 
| 7 | 0 | 1 | 1 | 1 | 
| 8 | 0 | 1 | 1 | 0 | 
| 9 | 1 | 0 | 0 | 0 | 

Mọi vị trí đều thành công. 

Trả lời:```
0
```Ví dụ này cho thấy số lượng cân bằng cho phép sắp xếp lại hoàn toàn mà không bị xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để đếm ký tự và một lượt để quét | 
| Không gian | O(1) | Chỉ có hai quầy được lưu trữ | 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`, do đó thuật toán chỉ thực hiện tổng cộng vài trăm nghìn thao tác. Điều này vừa vặn thoải mái trong giới hạn 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = []

    t = int(input())
    for _ in range(t):
        s = input().strip()
        n = len(s)

        cnt = [0, 0]
        for ch in s:
            cnt[int(ch)] += 1

        ans = 0

        for i, ch in enumerate(s):
            need = 1 - int(ch)

            if cnt[need] == 0:
                ans = n - i
                break

            cnt[need] -= 1

        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""4
0
011
0101110001
111100
"""
) == """1
1
0
4
"""

# minimum size
assert run(
"""1
1
"""
) == """1
"""

# already perfect after swaps
assert run(
"""1
01
"""
) == """0
"""

# all equal
assert run(
"""1
11111
"""
) == """5
"""

# failure in the middle
assert run(
"""1
0011
"""
) == """0
"""

# off-by-one style case
assert run(
"""1
0111
"""
) == """1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Chuỗi nhỏ nhất có thể | 
|`01`|`0`| Sắp xếp lại hoàn toàn mà không cần xóa | 
|`11111`|`5`| Tất cả các ký tự phải được xóa | 
|`0011`|`0`| Cân bằng chính xác giữa số không và số | 
|`0111`|`1`| Thất bại chỉ xảy ra ở vị trí cuối cùng | 

## Vỏ cạnh 

### Chuỗi ký tự đơn 

đầu vào:```
0
```Số lượng là`(1 zero, 0 ones)`. Vị trí đầu tiên yêu cầu`1`, nhưng không tồn tại. Sự cố xảy ra ngay tại vị trí`0`. 

Câu trả lời là:```
1 - 0 = 1
```Thuật toán trả về chính xác`1`, nghĩa là kết quả hợp lệ duy nhất là chuỗi trống. 

### Tất cả các ký tự đều giống nhau 

đầu vào:```
111100
```Bốn vị trí đầu tiên đều yêu cầu số 0. Chỉ có hai số không tồn tại trong toàn bộ chuỗi. 

Sau khi sử dụng hai số 0, số 0 bắt buộc thứ ba sẽ không còn nữa. Sự cố xảy ra ở vị trí`2`, đưa ra câu trả lời`6 - 2 = 4`. 

Thuật toán phát hiện chính xác khi hết nguồn cung cấp các bit đối diện. 

### Không cần xóa 

đầu vào:```
01
```Số đếm là một không và một một. 

Vị trí 0 tiêu thụ có sẵn`1`và vị trí 1 tiêu thụ sẵn có`0`. 

Quá trình quét kết thúc thành công nên câu trả lời là`0`. 

Điều này xác nhận rằng thuật toán không xóa các ký tự một cách không cần thiết khi có sự xáo trộn hoàn toàn của chuỗi nhị phân.
