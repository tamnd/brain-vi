---
title: "CF 102375D - \u0414\u0440\u0430\u0444\u0442 \u041d\u0411\u0410"
description: "Đối với mỗi ứng cử viên, chúng tôi biết năm số liệu thống kê số nguyên: chiều cao, sải cánh, số điểm mỗi trận, số lần bật bóng mỗi trận và kiến ​​​​tạo mỗi trận. Mỗi thống kê có khoảng thời gian dự kiến ​​riêng và ứng viên được đánh giá theo vị trí của mọi giá trị so với khoảng đó."
date: "2026-08-12T22:23:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "D"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 1135
verified: true
draft: false
---

[CF 102375D - \u0414\u0440\u0430\u0444\u0442 \u041d\u0411\u0410](https://codeforces.com/problemset/problem/102375/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 18m 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi ứng cử viên, chúng tôi biết năm số liệu thống kê số nguyên: chiều cao, sải cánh, số điểm mỗi trận, số lần bật bóng mỗi trận và kiến ​​tạo mỗi trận. Mỗi thống kê có khoảng thời gian dự kiến ​​riêng và ứng viên được đánh giá theo vị trí của mọi giá trị so với khoảng đó. 

Đối với một giá trị nằm trong khoảng dự kiến, điểm giữa thuộc về nửa trên. Vì vậy, năm ngưỡng để ở nửa trên là 205 cho chiều cao, 225 cho sải cánh, 15 cho điểm, 4 cho rebounds và 5 cho hỗ trợ. Giá trị hoàn toàn lớn hơn điểm cuối trên được coi là nằm trên phạm vi dự kiến. Ví dụ: chiều cao 205 đã ở nửa trên, trong khi chiều cao 221 nằm trên phạm vi mong đợi. 

Nhiệm vụ là phân loại từng ứng viên vào loại 0, 1, 2 hoặc 3. Loại 0 có điều kiện mạnh nhất và phải được kiểm tra trước. Nó yêu cầu ít nhất ba số liệu thống kê trên phạm vi mong đợi của họ, với chiều cao hoặc sải cánh nằm trong số ba số liệu đó. Tiếp theo là loại 1, tiếp theo là loại 2. Nếu không có điều kiện nào thỏa mãn thì câu trả lời là loại 3. 

Dữ liệu đầu vào chứa tối đa 32 ứng viên và mỗi ứng viên luôn có chính xác năm giá trị. Ngay cả một thuật toán thực hiện vài trăm thao tác cho mỗi ứng viên cũng sẽ nhanh chóng. Không cần sắp xếp, lập trình động, thuật toán đồ thị hoặc bất kỳ cấu trúc dữ liệu nào. Mục tiêu hữu ích là công việc liên tục cho mỗi ứng viên, mang lại tổng thời gian là O(N). 

Một ranh giới tinh tế là điểm giữa thuộc về nửa trên. Đối với chiều cao, khoảng dự kiến ​​là 190 đến 220, vì vậy 205 được tính là nửa trên. Thí sinh có chiều cao 204 không thỏa mãn điều kiện đó. Ví dụ:```
1
205
225
15
4
5
```Cả năm số liệu thống kê ít nhất đều nằm ở nửa trên nên đáp án là`2`, không`3`. Việc triển khai bằng cách sử dụng nghiêm ngặt`>`so sánh với điểm giữa sẽ bác bỏ không chính xác tất cả năm điều kiện ở nửa trên. 

Bản thân điểm cuối phía trên không nằm trên phạm vi mong đợi. Về chiều cao, 220 vẫn nằm trong phạm vi dự kiến, trong khi 221 vẫn ở trên mức đó. Ví dụ:```
1
220
250
20
6
7
```Tất cả năm giá trị đều nằm trong phạm vi mong đợi và tất cả đều nằm ở nửa trên của chúng, vì vậy câu trả lời là`2`. Việc coi điểm cuối trên là "ở trên" sẽ tạo ra ba hoặc nhiều số liệu thống kê trên phạm vi không chính xác. 

Một sai lầm dễ mắc phải khác là quên yêu cầu đặc biệt về chiều cao hoặc sải cánh đối với loại 0. Hãy xem xét:```
1
200
210
21
7
8
```Điểm, số lần bật lại và hỗ trợ đều nằm trên phạm vi mong đợi của họ, nhưng cả chiều cao và sải cánh đều không vượt quá phạm vi của nó. Câu trả lời đúng là`1`, bởi vì ứng viên thỏa mãn điều kiện vòng một, nhưng không thỏa mãn điều kiện kỳ ​​lân. Chỉ kiểm tra số lượng thống kê trên phạm vi sẽ xuất ra không chính xác`0`. 

Cuối cùng, các danh mục được phân cấp. Một ứng viên đáp ứng loại 0 cũng có thể đáp ứng các điều kiện yếu hơn từ loại 1 hoặc 2, vì vậy loại 0 phải được kiểm tra trước. Ví dụ:```
1
230
260
21
7
5
```Chiều cao, sải cánh và các điểm nằm trên phạm vi của chúng và chiều cao là một trong số đó. Câu trả lời là`0`, mặc dù ứng viên cũng đáp ứng điều kiện loại 1. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu theo nghĩa đen có thể phân loại từng số liệu thống kê trong số năm số liệu thống kê thành một trong bốn trạng thái: dưới khoảng dự kiến, ở nửa dưới, ở nửa trên hoặc trên khoảng dự kiến. Chỉ có (4^5=1024) kết hợp trạng thái có thể có cho một ứng cử viên. Chúng ta có thể liệt kê tất cả chúng, xác định loại mà mỗi sự kết hợp nhận được và so sánh ứng viên với những trường hợp đó. Đối với đầu vào tối đa là 32 ứng cử viên, đây chỉ là kiểm tra trạng thái (32\cdot1024=32768), do đó, ngay cả phương pháp ngây thơ có chủ ý này cũng dễ dàng đủ nhanh cho các ràng buộc thực tế. 

Nó trở nên kém hấp dẫn khi số lượng ứng viên tăng lên, bởi vì công việc có số lượng thống kê theo cấp số nhân. Với năm số liệu thống kê cố định, sự khác biệt này chủ yếu là về mặt lý thuyết, nhưng nếu cùng một ý tưởng được khái quát hóa thành 20 số liệu thống kê, thì việc liệt kê sẽ yêu cầu (4^{20}), tức là khoảng (1.1\cdot10^{12}) kết hợp. Ngay cả đối với bài toán thống kê năm hiện tại, không có lý do gì để liệt kê các trạng thái khi các quy tắc phân loại chỉ phụ thuộc vào một vài số đếm. 

Quan sát quan trọng là các giá trị chính xác không quan trọng sau khi chúng tôi xác định hai thuộc tính cho mỗi thống kê. Chúng ta chỉ cần biết liệu nó có nằm ở nửa trên hay không và liệu nó có vượt quá phạm vi mong đợi hay không. Chúng ta có thể đếm trực tiếp các thuộc tính này. 

Đối với mỗi ứng cử viên, hãy`high`là số lượng số liệu thống kê ít nhất ở nửa trên và đặt`above`là con số hoàn toàn vượt quá phạm vi dự kiến. Chúng tôi cũng ghi nhớ riêng xem chiều cao hoặc sải cánh có vượt quá phạm vi dự kiến ​​hay không. Mọi điều kiện danh mục sau đó có thể được thể hiện bằng cách sử dụng các số đếm này. 

Ngoài ra, quy tắc ở vòng đầu tiên còn có cụm từ "tất cả các tham số ít nhất đều nằm trong phạm vi dự kiến". Điều này tương đương với việc nói rằng không có số liệu thống kê nào trong số năm số liệu thống kê nằm dưới điểm cuối thấp hơn. Chúng ta có thể theo dõi điều đó bằng một biến boolean. Khi có sẵn một số bộ đếm này, mọi danh mục đều có thể được kiểm tra trực tiếp. 

Cách tiếp cận vũ phu hoạt động vì không gian trạng thái rất nhỏ, nhưng nó thực hiện công việc trên các trạng thái mà ứng viên thực sự không bao giờ đạt tới. Nhận xét rằng mọi quy tắc chỉ phụ thuộc vào một số ít các thuộc tính tổng hợp sẽ làm giảm vấn đề thành một số lượng so sánh không đổi cho mỗi ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N\cdot4^5)) | (O(1)) | Được chấp nhận cho các ràng buộc nhất định, nhưng đắt tiền không cần thiết | 
| Tối ưu | (O(N)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc năm số liệu thống kê của một ứng cử viên. Chúng tôi xử lý các ứng viên một cách độc lập vì hạng mục của một ứng viên không bao giờ phụ thuộc vào ứng viên khác. 
2. Đối với mỗi thống kê, hãy xác định xem liệu nó có ít nhất là ngưỡng nửa trên hay không và liệu nó có vượt quá điểm cuối dự kiến ​​hay không. Các ngưỡng lần lượt là 205, 225, 15, 4 và 5. Sự khác biệt giữa`>=`Và`>`là cần thiết vì điểm giữa thuộc nửa trên, trong khi điểm cuối phía trên vẫn nằm trong khoảng dự kiến. 
3. Đếm xem có bao nhiêu số liệu thống kê ở nửa trên hoặc phía trên nó. Gọi đây`high`. Đồng thời đếm xem có bao nhiêu số liệu thống kê vượt quá phạm vi mong đợi của chúng. Gọi đây`above`. 
4. Ghi lại xem chiều cao hoặc sải cánh có vượt quá phạm vi dự kiến ​​hay không. Điều này là cần thiết vì loại 0 yêu cầu một trong hai phép đo vật lý đó nằm trong số thống kê trên phạm vi. 
5. Đầu tiên hãy kiểm tra danh mục 0. Nó giữ nguyên khi`above >= 3`và chiều cao hoặc sải cánh vượt quá phạm vi mong đợi của nó. Chúng tôi kiểm tra nó trước vì kỳ lân cũng đáp ứng các điều kiện chất lượng yếu hơn, nhưng loại 0 được ưu tiên hơn. 
6. Nếu loại 0 không áp dụng được, hãy chọn loại 1. Điều kiện đầu tiên của nó là`above >= 2`cùng với`high >= 3`. Điều kiện thứ hai của nó yêu cầu mọi số liệu thống kê ít nhất phải nằm trong phạm vi dự kiến ​​và ít nhất ba số liệu thống kê nằm ở nửa trên. Điều kiện đầu tiên đủ để biểu thị "hai ở trên và nửa còn lại ít nhất là nửa trên", bởi vì mọi giá trị trên phạm vi đều nằm trong nhóm nửa trên hoặc cao hơn. 
7. Nếu loại 1 không áp dụng được, hãy chọn loại 2. Điều kiện đầu tiên của nó là`above >= 1`Và`high >= 2`. Điều kiện thứ hai của nó chỉ đơn giản là`high >= 3`. Một lần nữa, giá trị trên phạm vi dự kiến ​​sẽ tự động được tính bằng`high`. 
8. Nếu không có điều kiện nào trước đó được thỏa mãn, xuất ra loại 3. Vì các danh mục được kiểm tra theo thứ tự ưu tiên cần thiết nên đây chính xác là trường hợp còn lại. 

### Tại sao nó hoạt động 

Đối với mỗi ứng cử viên,`above`chính xác là số lượng thống kê cao hơn điểm cuối dự kiến ​​của họ, trong khi`high`chính xác là số lượng thống kê ít nhất ở nửa trên của họ. Cờ vật lý riêng biệt nắm bắt chính xác yêu cầu về chiều cao hoặc sải cánh bổ sung cho loại 0. Do đó, mỗi điều kiện bằng văn bản tương đương với quy tắc tương ứng trong hệ thống phân loại. Vì danh mục 0 được kiểm tra trước danh mục 1, danh mục 1 trước danh mục 2 và danh mục 3 chỉ được sử dụng khi không có danh mục nào phù hợp nên danh mục được tạo ra là danh mục có khả năng áp dụng cao nhất cho mọi ứng viên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def classify(values):
    lower = [190, 200, 10, 2, 3]
    upper = [220, 250, 20, 6, 7]
    middle = [205, 225, 15, 4, 5]

    high = 0
    above = 0
    all_expected = True

    for i, x in enumerate(values):
        if x < lower[i]:
            all_expected = False

        if x >= middle[i]:
            high += 1

        if x > upper[i]:
            above += 1

    physical_above = values[0] > upper[0] or values[1] > upper[1]

    if above >= 3 and physical_above:
        return 0

    if (above >= 2 and high >= 3) or (all_expected and high >= 3):
        return 1

    if (above >= 1 and high >= 2) or high >= 3:
        return 2

    return 3

def solve():
    n = int(input())
    answer = []

    for _ in range(n):
        values = [int(input()) for _ in range(5)]
        answer.append(str(classify(values)))

    sys.stdout.write("\n".join(answer))

if __name__ == "__main__":
    solve()
```Các mảng`lower`,`upper`, Và`middle`lưu trữ năm ranh giới theo cùng thứ tự với các giá trị đầu vào. Việc giữ các ngưỡng được căn chỉnh rõ ràng với năm số liệu thống kê sẽ làm cho logic phân loại trở nên thống nhất thay vì yêu cầu năm đoạn mã so sánh riêng biệt.`all_expected`bắt đầu là đúng và trở thành sai ngay khi giá trị giảm xuống dưới khoảng dự kiến ​​của nó. Điều này trực tiếp thể hiện điều kiện loại 1 thứ hai, yêu cầu mọi thống kê ít nhất phải là điểm cuối thấp hơn trong phạm vi dự kiến ​​của nó. 

biểu hiện`x >= middle[i]`đếm nửa trên, bao gồm cả điểm giữa. biểu hiện`x > upper[i]`chỉ đếm các giá trị nằm ngoài phạm vi dự kiến ​​phía trên nó. Những so sánh này không được hoán đổi hoặc thực hiện nghiêm ngặt theo hướng sai lầm. 

các`physical_above`điều kiện chỉ kiểm tra chiều cao và sải cánh, vì quy tắc kỳ lân đặc biệt yêu cầu một trong hai số liệu thống kê đó phải nằm trên phạm vi của nó. Chúng ta không cần biết cái nào nếu có ít nhất một cái thỏa mãn điều kiện. 

Không có vấn đề tràn số nguyên trong Python và ngay cả trong ngôn ngữ có số nguyên có chiều rộng cố định, tất cả các bộ đếm ở đây đều có tối đa là năm. Dữ liệu đầu vào chứa chính xác năm dòng cho mỗi ứng viên, do đó khả năng hiểu danh sách sẽ đọc chính xác các giá trị thuộc về ứng viên hiện tại. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, năm ngưỡng được cố định như mô tả ở trên. Bảng sau ghi lại các bộ đếm quan trọng cho mỗi ứng cử viên. 

| Người chơi | Giá trị |`high`|`above`| Chiều cao/Sải cánh trên | Tất cả đều nằm trong phạm vi mong đợi | Danh mục | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 230, 190, 16, 7, 9 | 4 | 3 | Có | Không | 0 | 
| 2 | 205, 225, 15, 5, 2 | 4 | 0 | Không | Không | 2 | 
| 3 | 210, 210, 30, 9, 9 | 5 | 3 | Không | Không | 1 | 

Đối với người chơi 1, chiều cao, số điểm, số lần bật lại và hỗ trợ nằm ở nhóm nửa trên hoặc cao hơn, trong khi chiều cao, số lần bật lại và hỗ trợ đều vượt quá phạm vi mong đợi của họ. Vì chiều cao là một trong những giá trị nằm trên phạm vi đó nên loại 0 sẽ được áp dụng ngay lập tức. 

Người chơi 2 có bốn giá trị ở nửa trên nhưng hỗ trợ bằng 2, thấp hơn khoảng thời gian dự kiến. Ứng viên không có số liệu thống kê trên phạm vi, vì vậy cả điều kiện loại 0 và điều kiện loại 1 đầu tiên đều không thể đáp ứng. Ba giá trị nửa trên trở lên là đủ cho loại 2, đưa ra câu trả lời`2`. 

Người chơi 3 có ba giá trị trên phạm vi mong đợi của họ, đó là điểm, số lần bật lại và hỗ trợ. Chiều cao và sải cánh không vượt quá phạm vi của chúng, vì vậy loại 0 không thành công. Điều kiện loại 1 đầu tiên đúng vì có ít nhất hai giá trị trên phạm vi và ít nhất ba giá trị nửa trên hoặc cao hơn, cho`1`. 

Ví dụ thứ hai tập trung vào các giá trị biên và điều kiện kỳ ​​lân đặc biệt.```
4
205
225
15
4
5
220
250
20
6
7
221
251
21
7
8
200
210
21
7
8
```| Người chơi |`high`|`above`| Chiều cao/Sải cánh trên | Tất cả đều nằm trong phạm vi mong đợi | Danh mục | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | Không | Có | 2 | 
| 2 | 5 | 0 | Không | Có | 2 | 
| 3 | 5 | 5 | Có | Không | 0 | 
| 4 | 3 | 3 | Không | Không | 1 | 

Hai người chơi đầu tiên chứng minh rằng cả điểm giữa và điểm cuối trên đều được tính là một phần của phạm vi dự kiến. Mọi số liệu thống kê đều nằm ở nửa trên, vì vậy loại 2 được áp dụng thông qua`high >= 3`luật lệ. 

Người chơi 3 có tất cả năm chỉ số trên phạm vi mong đợi của họ và chiều cao là một trong số đó, vì vậy hạng mạnh nhất là 0. 

Người chơi 4 có ba số liệu thống kê trên phạm vi, nhưng không có số liệu thống kê thể chất nào vượt quá phạm vi dự kiến. Do đó, quy tắc kỳ lân không thành công, trong khi quy tắc vòng đầu tiên thành công vì có ít nhất hai giá trị trên phạm vi và ít nhất ba giá trị nửa trên hoặc cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Mỗi (N) ứng cử viên có chính xác năm số liệu thống kê, vì vậy việc phân loại mất nhiều công sức | 
| Không gian | (O(1)) phụ trợ | Chỉ cần năm giá trị và một vài bộ đếm cho ứng viên hiện tại | 

Với (N\le32), thuật toán chỉ thực hiện vài trăm phép so sánh cơ bản. Nó thấp hơn nhiều so với bất kỳ giới hạn thời gian hoặc bộ nhớ thực tế nào và cấu trúc tuyến tính của nó sẽ vẫn hiệu quả ngay cả khi số lượng ứng viên tăng lên đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def classify(values):
    lower = [190, 200, 10, 2, 3]
    upper = [220, 250, 20, 6, 7]
    middle = [205, 225, 15, 4, 5]

    high = 0
    above = 0
    all_expected = True

    for i, x in enumerate(values):
        if x < lower[i]:
            all_expected = False
        if x >= middle[i]:
            high += 1
        if x > upper[i]:
            above += 1

    physical_above = values[0] > upper[0] or values[1] > upper[1]

    if above >= 3 and physical_above:
        return 0

    if (above >= 2 and high >= 3) or (all_expected and high >= 3):
        return 1

    if (above >= 1 and high >= 2) or high >= 3:
        return 2

    return 3

def solve():
    input = sys.stdin.readline
    n = int(input())
    result = []

    for _ in range(n):
        values = [int(input()) for _ in range(5)]
        result.append(str(classify(values)))

    return "\n".join(result)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided sample
assert run("""3
230
190
16
7
9
205
225
15
5
2
210
210
30
9
9
""") == """0
2
1""", "sample 1"

# Minimum-size input, every value below its expected range
assert run("""1
189
199
9
1
2
""") == "3", "all values below expected"

# All values exactly at the upper-half midpoint
assert run("""1
205
225
15
4
5
""") == "2", "midpoints belong to upper half"

# All values exactly at the upper endpoint
assert run("""1
220
250
20
6
7
""") == "2", "upper endpoints are not above range"

# Three above-range statistics, but neither physical statistic is above
assert run("""1
200
210
21
7
8
""") == "1", "no physical statistic above range"

# Genuine unicorn, with exactly three above-range values
assert run("""1
221
250
21
6
7
""") == "0", "height plus two skills above range"

# Maximum-size input, all candidates identical
maximum_case = "32\n" + "\n".join(
    ["205", "225", "15", "4", "5"] * 32
) + "\n"
assert run(maximum_case) == "\n".join(["2"] * 32), "maximum N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`189, 199, 9, 1, 2`|`3`| Đầu vào có kích thước tối thiểu và các giá trị dưới mọi phạm vi dự kiến ​​| 
|`205, 225, 15, 4, 5`|`2`| Điểm giữa được bao gồm ở nửa trên | 
|`220, 250, 20, 6, 7`|`2`| Điểm cuối trên không vượt quá phạm vi dự kiến ​​| 
|`200, 210, 21, 7, 8`|`1`| Ba kỹ năng tầm cao không làm nên kỳ lân nếu không có chiều cao hay sải cánh | 
|`221, 250, 21, 6, 7`|`0`| Chính xác ba giá trị trên phạm vi có chiều cao trên phạm vi | 
| 32 thí sinh giống hệt nhau | 32 dòng`2`| Kích thước đầu vào tối đa và xử lý độc lập | 

## Vỏ cạnh 

Ranh giới trung điểm được xử lý bằng cách sử dụng`>=`. Đối với đầu vào```
1
205
225
15
4
5
```tất cả năm giá trị đều thỏa mãn ngưỡng nửa trên của chúng, vì vậy`high = 5`. Không có giá trị nào vượt quá điểm cuối dự kiến ​​của nó, vì vậy`above = 0`. Loại 2 theo sau từ`high >= 3`, sản xuất`2`. Một sự so sánh chặt chẽ như`x > middle[i]`sẽ loại trừ không chính xác mọi điểm giữa. 

Ranh giới điểm cuối phía trên được xử lý riêng biệt với`>`. Vì```
1
220
250
20
6
7
```tất cả năm giá trị vẫn nằm trong phạm vi mong đợi của chúng. Họ cũng đều ở nửa trên, vì vậy`high = 5`Và`above = 0`. Kết quả là`2`. Điều này ngăn ngừa lỗi phổ biến khi coi điểm cuối trên được mong đợi là giá trị trên phạm vi. 

Yêu cầu thống kê vật lý được kiểm tra độc lập với tổng số. Vì```
1
200
210
21
7
8
```số điểm, số lần bật lại và hỗ trợ đều ở trên phạm vi của họ, mang lại`above = 3`. Chiều cao và sải cánh không vượt quá phạm vi của chúng, vì vậy`physical_above`là sai. Loại 0 bị từ chối, trong khi`above >= 2`Và`high >= 3`làm cho loại 1 hợp lệ. Đầu ra là`1`. 

Mức độ ưu tiên được xử lý theo thứ tự của các điều kiện. Vì```
1
221
250
21
6
7
```chiều cao, số điểm và số lần hỗ trợ đều vượt quá phạm vi của họ, vì vậy`above = 3`, và chiều cao làm cho`physical_above`ĐÚNG VẬY. Thuật toán trả về danh mục 0 ngay lập tức. Nó không bao giờ đạt đến các quy tắc loại 1 hoặc loại 2 yếu hơn, điều này là cần thiết vì đầu ra được yêu cầu là phân loại có thể áp dụng mạnh nhất.
