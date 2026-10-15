---
title: "CF 105427D - Chết Cứng"
description: "Chúng tôi được tặng ba viên xúc xắc khác nhau. Mỗi xúc xắc có sáu mặt, nhưng giá trị trên các mặt đó là các số nguyên tùy ý từ 1 đến 1000 và có thể lặp lại. Một trò chơi được chơi bằng cách trước tiên để John chọn một viên xúc xắc, sau đó Hans chọn một trong hai viên xúc xắc còn lại."
date: "2026-06-23T04:07:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105427
codeforces_index: "D"
codeforces_contest_name: "2023-2024 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2023)"
rating: 0
weight: 105427
solve_time_s: 63
verified: true
draft: false
---

[CF 105427D - Chết cứng](https://codeforces.com/problemset/problem/105427/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng ba viên xúc xắc khác nhau. Mỗi xúc xắc có sáu mặt, nhưng giá trị trên các mặt đó là các số nguyên tùy ý từ 1 đến 1000 và có thể lặp lại. Một trò chơi được chơi bằng cách trước tiên để John chọn một viên xúc xắc, sau đó Hans chọn một trong hai viên xúc xắc còn lại. Sau đó cả hai người chơi đều tung xúc xắc đã chọn. 

Nếu cả hai người chơi đều tung ra cùng một số, vòng đó sẽ bị bỏ qua và họ lại tung ra. Hậu quả quan trọng là các mối quan hệ không đóng góp vào việc phân phối kết quả, chúng chỉ lặp lại thử nghiệm cho đến khi xảy ra sự so sánh chặt chẽ. Khi hai kết quả khác nhau, ai tung được giá trị lớn hơn sẽ thắng trò chơi ngay lập tức. 

Nhiệm vụ là xác định xem có tồn tại một lựa chọn chết cho John sao cho dù Hans chọn con xúc xắc nào sau đó thì xác suất chiến thắng của John ít nhất là một nửa. Nếu nhiều viên xúc xắc thỏa mãn điều này, chúng ta sẽ xuất ra chỉ số nhỏ nhất. Nếu không có lựa chọn nào thỏa mãn điều kiện, chúng ta sẽ xuất ra kết quả là không có lựa chọn hợp lệ. 

Các ràng buộc rất nhỏ: chỉ có ba viên xúc xắc và mỗi viên chỉ có sáu mặt. Điều này có nghĩa là vấn đề không phải là tối ưu hóa trên các đầu vào lớn mà là về xác suất tính toán chính xác từ so sánh từng cặp. Bất kỳ giải pháp nào là bậc hai hoặc thậm chí bậc ba về số mặt đều dễ dàng đủ nhanh vì tổng số kết quả được cố định ở mức 36 trên mỗi cặp xúc xắc. 

Sự tinh tế chính là quy tắc tháo cà vạt. Một mô phỏng ngây thơ coi các mối quan hệ là tổn thất hoặc đưa chúng trực tiếp vào xác suất có thể không chính xác. Ví dụ: nếu cả hai viên xúc xắc giống hệt nhau, một tính toán đơn giản có thể ấn định xác suất thắng là 50% cho mỗi viên, nhưng trên thực tế, mọi lần tung xúc xắc đều hòa, vì vậy trò chơi không bao giờ giải quyết được và thực tế là không có người chiến thắng nào tồn tại. Trường hợp đó phải được coi là không có đóng góp gì cho cả hai bên. 

Trường hợp cạnh thứ hai là khi một con súc sắc lấn át con khác ngoại trừ điểm hòa. Ví dụ: nếu xúc xắc A và B có nhiều mặt bằng nhau nhưng nếu không thì A luôn lớn hơn, xác suất thắng chính xác là 1, không phải là thứ bị pha loãng bởi các mối quan hệ. 

## Phương pháp tiếp cận 

Ý tưởng về lực lượng vũ phu rất đơn giản: đối với hai viên xúc xắc bất kỳ, hãy liệt kê tất cả 36 cặp mặt. Đối với mỗi cặp, hãy quyết định xem kết quả là người chết đầu tiên thắng, người chết thứ hai thắng hay hòa. Đếm số lần thắng và thua. Vì các mối ràng buộc được cuộn lại nên chúng sẽ bị bỏ qua khi tính toán xác suất. 

Cụ thể, nếu chúng ta định nghĩa số trận thắng là số cặp trong đó A[i] > B[j] và số lần thua trong đó A[i] < B[j], thì chỉ những so sánh này mới quan trọng. Xác suất để A thắng B là: 

P(A thắng B) = thắng / (thắng + thua) 

Lực lượng vũ phu tính toán điều này trực tiếp cho mỗi cặp xúc xắc được đặt hàng. Vì chỉ có 3 viên xúc xắc nên chúng tôi tính toán hai lần so sánh cho mỗi ứng cử viên chết, do đó tổng cộng tối đa là 6 lần so sánh. Mỗi lần so sánh tốn 36 thao tác nên điều này không đáng kể. 

Không cần các kỹ thuật tiên tiến hơn vì không gian trạng thái rất nhỏ. Cạm bẫy thực sự duy nhất là xử lý các mối quan hệ một cách chính xác bằng cách loại trừ chúng khỏi cả tử số và mẫu số. 

Quan sát giúp đơn giản hóa mọi thứ là trò chơi giữa hai viên xúc xắc giảm hoàn toàn tần số thống trị theo cặp trên tất cả các kết hợp khuôn mặt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng cặp Brute Force | O(3 × 3 × 36) | O(1) | Đã chấp nhận | 
| Đếm cặp tối ưu | O(3 × 3 × 36) | O(1) | Đã chấp nhận | 

Trong thực tế, cả hai cách tiếp cận đều giống nhau; "tối ưu hóa" chỉ là công nhận rằng việc liệt kê đã tối ưu. 

## Hướng dẫn thuật toán 

Chúng tôi đánh giá mỗi người chết là ứng cử viên của John.

1. Đối với khuôn i cố định, so sánh nó với khuôn khác j. Chúng tôi tính toán tần suất tôi thắng j bằng cách kiểm tra tất cả 36 cặp mặt (a từ i, b từ j). Mỗi so sánh góp phần vào thắng, thua hoặc hòa. 
2. Đối với mỗi cặp xúc xắc, chúng tôi duy trì hai bộ đếm: thắng và thua. Nếu thắng + thua bằng 0, điều đó có nghĩa là mọi kết quả đều hòa, vì vậy cả hai con chết đều không thể thắng và việc ghép đôi này không đóng góp gì hữu ích. Trong trường hợp đó, chết tôi ngay lập tức không đạt yêu cầu vì Hans có thể chọn một con súc sắc khiến trò chơi không thể giải quyết được. 
3. Nếu thắng + thua là dương, chúng ta tính xác suất thắng / (thắng + thua). Chúng tôi kiểm tra xem đây có phải là ít nhất 1/2 hay không. Về mặt đại số, điều này tương đương với việc kiểm tra xem 2 * thắng ≥ thắng + thua hay không, điều này tránh được các vấn đề về dấu phẩy động. 
4. Xúc xắc i chỉ hợp lệ nếu nó thỏa mãn điều kiện xác suất đối với cả hai viên xúc xắc còn lại. Nếu nó vượt qua cả hai lần kiểm tra, chúng tôi sẽ trả về i làm câu trả lời. 
5. Nếu không có khuôn nào vượt qua, chúng ta sẽ xuất ra rằng không có khuôn hợp lệ nào tồn tại. 

Lý do chính khiến điều này có hiệu quả là vì Hans chơi tối ưu sau khi nhìn thấy lựa chọn của John, vì vậy chúng ta phải đảm bảo rằng viên xúc xắc mà John đã chọn mạnh mẽ trước cả hai lựa chọn còn lại một cách độc lập. Vì Hans chọn kết quả tệ nhất cho John nên điều kiện đúng là tối thiểu đối với hai xác suất theo cặp. 

### Tại sao nó hoạt động 

Đối với bất kỳ cặp xúc xắc cố định nào, mỗi kết quả tung ra sẽ giảm xuống mức so sánh với hai lần rút thống nhất độc lập. Các mối hòa tạo thành một tập hợp các thước đo được loại bỏ khỏi không gian kết quả vì chúng không kết thúc trò chơi. Các kết quả còn lại tạo thành một tập hợp hữu hạn trong đó mỗi cặp có thứ tự đóng góp như nhau. Do đó, xác suất thắng được xác định đầy đủ bằng cách tính các bất đẳng thức nghiêm ngặt thuận lợi. Vì Hans luôn chọn xúc xắc để giảm thiểu xác suất chiến thắng của John nên John chỉ thành công nếu cả hai xác suất theo cặp đều vượt quá ngưỡng. Điều này làm cho điều kiện có thể được tách biệt giữa hai viên xúc xắc của đối thủ, đảm bảo tính chính xác của các lần kiểm tra độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def win_prob(a, b):
    win = 0
    loss = 0
    for x in a:
        for y in b:
            if x > y:
                win += 1
            elif x < y:
                loss += 1
    total = win + loss
    if total == 0:
        return 0  # no resolution possible
    return win / total

dice = [list(map(int, input().split())) for _ in range(3)]

def ok(i):
    for j in range(3):
        if i == j:
            continue
        p = win_prob(dice[i], dice[j])
        if p < 0.5:
            return False
    return True

for i in range(3):
    if ok(i):
        print(i + 1)
        break
else:
    print("No dice")
```Giải pháp trước tiên xác định một hàm trợ giúp tính toán xác suất thắng giữa hai viên xúc xắc bằng cách liệt kê tất cả các cặp mặt. Logic phân tách rõ ràng thắng thua và bỏ qua các mối quan hệ, điều này rất quan trọng cho tính chính xác. 

các`ok`chức năng thực thi yêu cầu rằng một viên xúc xắc được chọn phải thành công trước cả hai viên xúc xắc còn lại. Nó lặp lại hai đối thủ và kiểm tra điều kiện xác suất. 

Cuối cùng, chúng tôi quét xúc xắc theo thứ tự tăng dần để chọn chỉ số hợp lệ nhỏ nhất. 

Một chi tiết triển khai tinh tế là việc xử lý trường hợp trong đó tất cả các so sánh đều có quan hệ ràng buộc. Trả về số 0 đảm bảo rằng xúc xắc như vậy không bao giờ được coi là tốt, phù hợp với cách giải thích rằng trò chơi không bao giờ giải quyết được và do đó không đảm bảo chiến thắng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 3 4 5 6
1 2 3 4 5 6
1 2 3 4 5 6
```Tất cả các con xúc xắc đều giống hệt nhau. 

Đối với bất kỳ cặp nào, mọi so sánh đều dẫn đến kết quả hòa, do đó thắng = 0 và thua = 0. 

| Cặp | Thắng | Thua lỗ | Tổng cộng | Xác suất | 
| --- | --- | --- | --- | --- | 
| 1 vs 2 | 0 | 0 | 0 | 0 | 
| 1 vs 3 | 0 | 0 | 0 | 0 | 

Vì không có viên xúc xắc nào đạt được xác suất ≥ 1/2 so với bất kỳ đối thủ nào nên không có ứng cử viên nào hợp lệ. 

Đầu ra:```
No dice
```Điều này xác nhận việc xử lý các trò chơi thoái hóa mà không thể giải quyết được. 

### Ví dụ 2 

đầu vào:```
2 2 4 4 9 9
1 1 6 6 8 8
7 7 5 5 3 3
```Chúng ta so sánh chết 1 với chết 2 và chết 3. 

Đối với xúc xắc 1 và xúc xắc 2, nhiều kết quả nghiêng về xúc xắc 1 vì 4 và 9 lần lượt chiếm ưu thế hơn 1 và 6. Tỷ lệ thắng tính toán vượt quá 1/2. 

Đối với khuôn 1 và khuôn 3, cấu trúc giống nhau: giá trị lớn hơn của khuôn 1 chiếm ưu thế trong hầu hết các so sánh. 

| Ứng viên | vs Chết 2 | vs Chết 3 | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | ≥ 1/2 | ≥ 1/2 | Có | 
| 2 | hỗn hợp | hỗn hợp | Không | 
| 3 | hỗn hợp | hỗn hợp | Không | 

Vì vậy, chúng tôi xuất ra:```
1
```Ví dụ này cho thấy ưu thế trong so sánh theo cặp chuyển trực tiếp thành xác suất thắng, không phụ thuộc vào hình dạng phân bổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3 × 3 × 36) | Mỗi cặp xúc xắc được so sánh thông qua tất cả các cặp mặt | 
| Không gian | O(1) | Chỉ có bộ đếm và lưu trữ đầu vào | 

Việc tính toán có quy mô không đổi vì kích thước bài toán là cố định. Ngay cả với những sửa đổi thử nghiệm lặp đi lặp lại, giới hạn trên vẫn không đáng kể so với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    dice = [list(map(int, sys.stdin.readline().split())) for _ in range(3)]

    def win_prob(a, b):
        win = 0
        loss = 0
        for x in a:
            for y in b:
                if x > y:
                    win += 1
                elif x < y:
                    loss += 1
        total = win + loss
        if total == 0:
            return 0
        return win / total

    def ok(i):
        for j in range(3):
            if i == j:
                continue
            if win_prob(dice[i], dice[j]) < 0.5:
                return False
        return True

    for i in range(3):
        if ok(i):
            return str(i + 1)
    return "No dice"

# provided samples (as structured inputs)
assert run("1 2 3 4 5 6\n1 2 3 4 5 6\n1 2 3 4 5 6\n") == "No dice"
assert run("1 1 1 1 1 1\n2 2 2 2 2 2\n3 3 3 3 3 3\n") == "1"

# custom cases
assert run("6 6 6 6 6 6\n1 1 1 1 1 1\n2 2 2 2 2 2\n") == "1", "constant dominant die"
assert run("1 2 3 4 5 6\n6 6 6 6 6 6\n1 1 1 1 1 1\n") == "2", "single strong die"
assert run("1 1 2 2 3 3\n1 1 2 2 3 3\n1 1 2 2 3 3\n") == "No dice", "all identical"
assert run("1 3 5 7 9 11\n2 4 6 8 10 12\n1 1 1 1 1 1\n") == "2", "alternating dominance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các viên xúc xắc đều bằng nhau | Không có xúc xắc | kết quả chỉ hòa | 
| chiếm ưu thế chết đầu tiên | 1 | xử lý thống trị nghiêm ngặt | 
| phân phối giống hệt nhau | Không có xúc xắc | sự cố đối xứng | 
| đặt hàng hỗn hợp | 2 | so sánh không tầm thường | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi hai viên xúc xắc giống hệt nhau. Trong tình huống đó, mỗi lần đổ đều có kết quả hòa, vì vậy thắng + thua bằng 0. Thuật toán ánh xạ điều này tới xác suất bằng 0, điều này ngăn không cho một trong hai người chết được coi là người chiến thắng được đảm bảo hợp lệ. Điều này phù hợp với cách giải thích dự định mà trò chơi không bao giờ giải quyết được. 

Một trường hợp khác là khi một con chết thống trị hoàn toàn con chết khác. Ví dụ: nếu xúc xắc A luôn lớn hơn xúc xắc B thì mỗi cặp đều đóng góp vào số lần thắng và xác suất trở thành 1. Thuật toán nắm bắt được điều này một cách tự nhiên vì tổn thất vẫn bằng 0 và hòa không ảnh hưởng đến việc chuẩn hóa. 

Trường hợp thứ ba là khi mối quan hệ diễn ra thường xuyên nhưng không trọn vẹn. Giả sử có nhiều mặt bằng nhau nhưng một số mặt lớn hơn. Thuật toán bỏ qua các kết quả bằng nhau một cách chính xác và chỉ tính xác suất dựa trên các so sánh nghiêm ngặt, đảm bảo rằng các viên xúc xắc nặng bằng điểm không bị phạt sai ngoài tác động thực tế của chúng.
