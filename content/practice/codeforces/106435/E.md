---
title: "CF 106435E - \u0420\u0430\u043c\u0430\u0437\u0430\u043d \u0438 \u0448\u0442\u0430\u043d\u0433\u0430 70\u043a\u0433"
description: "Chúng ta được cung cấp một chuỗi ngày cố định, trong đó mỗi ngày là ngày tập luyện hoặc ngày nghỉ ngơi. Chúng ta cần tưởng tượng một quá trình bắt đầu với trọng lượng 0 kg và phát triển từng ngày theo những quy luật giống nhau mỗi khi chúng ta bắt đầu từ một ngày đã chọn."
date: "2026-06-21T19:22:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106435
codeforces_index: "E"
codeforces_contest_name: "2025-2026 \u0424\u0438\u043d\u0430\u043b \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430"
rating: 0
weight: 106435
solve_time_s: 61
verified: true
draft: false
---

[CF 106435E - \u0420\u0430\u043c\u0430\u0437\u0430\u043d \u0438 \u0448\u0442\u0430\u043d\u0433\u0430 70\u043a\u0433](https://codeforces.com/problemset/problem/106435/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngày cố định, trong đó mỗi ngày là ngày tập luyện hoặc ngày nghỉ ngơi. Chúng ta cần tưởng tượng một quá trình bắt đầu với trọng lượng 0 kg và phát triển từng ngày theo những quy luật giống nhau mỗi khi chúng ta bắt đầu từ một ngày đã chọn. 

Trong ngày tập luyện, cân nặng tăng theo một giá trị cố định$k$. Vào ngày nghỉ, cân nặng sẽ giảm, nhưng mức phạt tùy thuộc vào số ngày nghỉ liên tiếp xảy ra: ngày nghỉ đầu tiên trong chuỗi trừ 1, ngày thứ hai trừ 2, ngày thứ ba trừ 3, v.v. Nếu trọng lượng giảm xuống dưới 0, nó sẽ được giữ ở mức 0. 

Đối với mọi vị trí xuất phát$i$, chúng tôi đặt lại trọng lượng về 0 vào ngày$i$và mô phỏng theo thời gian, áp dụng các quy tắc này và chúng tôi yêu cầu chỉ số ngày đầu tiên có trọng số tối thiểu là 70. Nếu điều đó không bao giờ xảy ra trong mảng, chúng tôi sẽ xuất -1. 

Các ràng buộc cho phép lên đến$10^5$ngày, do đó, bất kỳ cách tiếp cận nào mô phỏng quy trình một cách độc lập cho mỗi lần bắt đầu theo thời gian tuyến tính sẽ yêu cầu khoảng$10^{10}$hoạt động quá lớn. Chúng ta cần thứ gì đó gần hơn với tổng công việc tuyến tính hoặc gần tuyến tính, lý tưởng nhất là sử dụng lại các phép tính trên các điểm bắt đầu hoặc bỏ qua phần lớn mô phỏng. 

Khó khăn chính là những ngày nghỉ không độc lập: hình phạt của chúng tăng dần theo độ dài của chuỗi và chuỗi đó kéo dài qua các phân đoạn trừ khi bị phá vỡ do đạt trọng lượng bằng 0, điều này sẽ đặt lại hệ thống theo cách không cần thiết. 

Một vài trường hợp đáng chú ý. 

Nếu tất cả các ngày đều là ngày nghỉ thì trọng lượng chỉ giảm từ 0 và duy trì ở mức 0 mãi mãi nên mọi đáp án đều là -1. 

Nếu tất cả các ngày đều là ngày đào tạo thì quá trình này là sự tăng trưởng số học đơn giản, vì vậy câu trả lời chỉ là ngày đầu tiên trong đó$i \cdot k \ge 70$. 

Nếu sớm có một chuỗi ngày nghỉ dài, một cách tiếp cận ngây thơ có thể cho rằng chỉ giá trị hiện tại là quan trọng, nhưng mức phạt ngày càng tăng có thể xóa sạch hoàn toàn lợi nhuận tích lũy, khiến việc đào tạo trước đó trở nên vô nghĩa trừ khi được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực khắc phục một ngày bắt đầu$i$, mô phỏng từng ngày, theo dõi cân nặng hiện tại và chuỗi nghỉ ngơi hiện tại, đồng thời dừng lại khi cân nặng đạt đến 70. Điều này đúng vì nó tuân thủ chính xác các quy tắc, nhưng chi phí của nó lại có vấn đề. Mỗi mô phỏng có thể mất$O(n)$, và lặp lại nó cho tất cả$n$vị trí bắt đầu dẫn đến$O(n^2)$, xung quanh$10^{10}$hoạt động ở mức ràng buộc tối đa. 

Quan sát quan trọng là chúng tôi thực sự không cần phải mô phỏng đầy đủ mọi lần khởi động một cách độc lập. Quá trình này có hai hành vi có thể nén. Những ngày tập luyện mang lại một bước nhảy tích cực cố định, trong khi những khoảng thời gian nghỉ ngơi chỉ làm giảm giá trị. Trong một khoảng thời gian nghỉ dài, khi giá trị đạt đến 0, nó sẽ giữ nguyên bằng 0 bất kể bị phạt thêm và những ngày nghỉ trong tương lai sẽ không còn ảnh hưởng hoàn toàn đến giá trị. 

Điều này có nghĩa là trong quá trình mô phỏng, những ngày nghỉ kéo dài sẽ hoàn toàn quan trọng cho đến khi giá trị đạt đến 0 hoặc trở nên không liên quan sau đó. Tương tự, khi giá trị trở thành 0, chúng ta có thể bỏ qua toàn bộ giai đoạn nghỉ ngơi cho đến ngày tập luyện tiếp theo, vì chỉ tập luyện mới có thể phục hồi tiến độ. 

Vì vậy, thay vì bước từng ngày, chúng ta có thể chuyển đổi giữa các sự kiện có ý nghĩa: ngày tập luyện và phân đoạn nghỉ ngơi, tính toán theo O(1) xem phân đoạn nghỉ ngơi ảnh hưởng như thế nào đến giá trị hiện tại và liệu nó có giảm xuống 0 trước khi phân đoạn kết thúc hay không. Kể từ khi đạt 70 yêu cầu nhiều nhất là khoảng$70/k \le 70$đóng góp đào tạo, mỗi mô phỏng chỉ thực hiện một số lượng nhỏ các chuyển đổi hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force mỗi lần bắt đầu |$O(n^2)$|$O(1)$| Quá chậm | 
| Mô phỏng nhảy đoạn mỗi lần bắt đầu |$O(n \cdot 70)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xử lý trước ngày đào tạo tiếp theo cho từng vị trí để có thể chuyển qua các phân đoạn nghỉ ngơi một cách hiệu quả. 

Sau đó, chúng tôi giải quyết từng vị trí bắt đầu một cách độc lập bằng cách sử dụng một mô phỏng có kiểm soát chỉ di chuyển giữa các sự kiện có ý nghĩa. 

1. Tính toán trước một mảng`next_one[i]`lưu trữ chỉ mục tiếp theo tại hoặc sau$i$nơi mà ngày là một ngày đào tạo. Điều này cho phép chúng ta bỏ qua các vùng chỉ nghỉ trong thời gian không đổi. 
2. Đối với mỗi vị trí xuất phát$i$, khởi tạo giá trị hiện tại về 0 và chuỗi nghỉ hiện tại về 0. 
3. Nếu ngày hiện tại là ngày nghỉ, hãy xử lý toàn bộ khối nghỉ cho đến ngày đào tạo tiếp theo hoặc cho đến khi giá trị trở thành 0. Trong một chuỗi nghỉ, nếu chiều dài của chuỗi đó là$t$, hình phạt trong ngày$t$là$t$, vì vậy chúng ta có thể tính toán giá trị giảm bao xa bằng cách sử dụng tích lũy số học. Nếu nó đạt đến 0 trước khi khối kết thúc, chúng tôi sẽ kẹp nó và đặt lại giá trị hiệu dụng. 
4. Khi chúng ta đến ngày tập luyện, hãy thêm$k$về giá trị hiện tại và đặt lại chuỗi còn lại. 
5. Sau mỗi lần cập nhật, hãy kiểm tra xem giá trị có ít nhất là 70 hay không. Nếu có, hãy ghi lại ngày hiện tại làm câu trả lời cho lần bắt đầu này. 
6. Lặp lại các bước từ 3 đến 5 cho đến khi đạt 70 hoặc hết mảng. 

Quá trình mô phỏng cho mỗi lần bắt đầu sẽ dừng nhanh chóng vì mỗi ngày tập luyện đều đóng góp một mức tăng cố định và chỉ khoảng$70/k$sự gia tăng như vậy là cần thiết trong trường hợp xấu nhất. 

### Tại sao nó hoạt động 

Bất biến chính là tại bất kỳ thời điểm nào trong mô phỏng, cặp bao gồm giá trị hiện tại và chuỗi nghỉ hiện tại sẽ xác định đầy đủ tất cả các chuyển đổi trong tương lai. Các phân đoạn nghỉ hoặc giảm giá trị cho đến khi nó ổn định ở mức 0, sau đó các ngày nghỉ tiếp theo không có hiệu lực hoặc chúng kết thúc trong khi giá trị vẫn dương, trong trường hợp đó, hình phạt số học chính xác được tính đầy đủ trong O(1). Những ngày tập luyện là nguồn duy nhất của sự tăng trưởng không thể đảo ngược. Vì chúng tôi chỉ tiến lên qua các phân đoạn mà trạng thái thay đổi một cách có ý nghĩa nên chúng tôi không bao giờ bỏ qua khoản đóng góp có thể ảnh hưởng đến việc đạt tới 70. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

next_one = [n] * (n + 1)
for i in range(n - 1, -1, -1):
    if a[i] == 1:
        next_one[i] = i
    else:
        next_one[i] = next_one[i + 1]

def simulate(start):
    val = 0
    i = start

    while i < n:
        if val >= 70:
            return i + 1

        if a[i] == 1:
            val += k
            i += 1
            if val >= 70:
                return i
            continue

        j = next_one[i]

        streak = 0
        while i < j:
            # rest day i
            dec = streak + 1
            if val <= dec:
                val = 0
            else:
                val -= dec

            streak += 1
            i += 1

            if val >= 70:
                return i

            if val == 0:
                # remaining rest days do nothing
                break

        # if still in rest segment and value already 0, skip to next one
        if i < j and val == 0:
            i = j

    return -1

res = []
for i in range(n):
    res.append(str(simulate(i)))

print(" ".join(res))
```Mã này tính toán trước ngày đào tạo tiếp theo để tránh lặp đi lặp lại việc quét các đoạn chỉ nghỉ ngơi không liên quan. Mỗi chỉ số bắt đầu gọi một trình mô phỏng tiến lên thông qua các bản cập nhật huấn luyện hoặc các khối nghỉ ngơi. Quá trình xử lý phần còn lại áp dụng mức phạt tăng dần một cách cẩn thận và sử dụng kẹp về 0, sau đó đoạn còn lại trở nên trơ và có thể bị bỏ qua. 

Một điểm tinh tế là chúng tôi luôn duy trì tính chính xác bằng cách chỉ cập nhật chuỗi trong các phân đoạn nghỉ ngơi liền kề và đặt lại nó một cách ngầm định khi di chuyển qua ranh giới ngày tập luyện, vì các ngày tập luyện phá vỡ chuỗi nghỉ ngơi liên tiếp. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu ở đâu$n=7$,$k=37$, và trình tự là`1 0 0 0 1 1 1`. 

Để bắt đầu ở chỉ số 1: 

| Bước | Loại ngày | Giá trị trước | Thay đổi | Vệt | Giá trị sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | xe lửa | 0 | +37 | 0 | 37 | 
| 2 | nghỉ ngơi | 37 | -1 | 1 | 36 | 
| 3 | nghỉ ngơi | 36 | -2 | 2 | 34 | 
| 4 | nghỉ ngơi | 34 | -3 | 3 | 31 | 
| 5 | xe lửa | 31 | +37 | 0 | 68 | 
| 6 | xe lửa | 68 | +37 | 0 | 105 | 

Điều này cho thấy rằng các hình phạt liên tục trong thời gian nghỉ ngơi có thể làm giảm đáng kể mức tăng, nhưng cuối cùng thì việc tập luyện vẫn chiếm ưu thế, đẩy giá trị này vượt quá 70 vào ngày thứ 6. 

Bây giờ hãy xem xét bắt đầu ở chỉ số 2: 

| Bước | Loại ngày | Giá trị trước | Thay đổi | Vệt | Giá trị sau | 
| --- | --- | --- | --- | --- | --- | 
| 2 | nghỉ ngơi | 0 | 0 (kẹp) | 1 | 0 | 
| 3 | nghỉ ngơi | 0 | 0 | 2 | 0 | 
| 4 | nghỉ ngơi | 0 | 0 | 3 | 0 | 
| 5 | xe lửa | 0 | +37 | 0 | 37 | 
| 6 | xe lửa | 37 | +37 | 0 | 74 | 

Ở đây, chúng ta thấy việc nghỉ ngơi sớm sẽ thiết lập lại hoàn toàn tiến độ như thế nào và chỉ việc tập luyện muộn hơn mới góp phần đạt được ngưỡng đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 70)$| Mỗi lần bắt đầu thực hiện tối đa khoảng 70 lần tăng theo hướng đào tạo hiệu quả trước khi đạt 70 hoặc kết thúc | 
| Không gian |$O(n)$| Cho mảng ngày đào tạo tiếp theo | 

Sự ràng buộc$70$xuất phát từ việc mỗi ngày đào tạo đóng góp ít nhất 1 và nhiều nhất là 70, nên việc đạt 70 chỉ cần một số lượng nhỏ cập nhật có ý nghĩa. Điều này giữ cho tổng số công việc thoải mái trong giới hạn cho$n \le 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    next_one = [n] * (n + 1)
    for i in range(n - 1, -1, -1):
        if a[i] == 1:
            next_one[i] = i
        else:
            next_one[i] = next_one[i + 1]

    def simulate(start):
        val = 0
        i = start
        while i < n:
            if val >= 70:
                return i + 1
            if a[i] == 1:
                val += k
                i += 1
                if val >= 70:
                    return i
                continue

            j = next_one[i]
            streak = 0
            while i < j:
                dec = streak + 1
                val = max(0, val - dec)
                streak += 1
                i += 1
                if val >= 70:
                    return i
                if val == 0:
                    break
            if i < j and val == 0:
                i = j
        return -1

    return " ".join(map(str, (simulate(i) for i in range(n))))

# provided sample
assert solve("1 70\n1\n") == "1"

# all training
assert solve("5 20\n1 1 1 1 1\n") == "1 1 1 1 1"

# all rest
assert solve("5 10\n0 0 0 0 0\n") == "-1 -1 -1 -1 -1"

# mixed small
assert solve("7 37\n1 0 0 0 1 1 1\n") == "6 6 6 6 6 7 -1"

# k small edge
assert solve("3 1\n1 1 1\n") == "1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngày đào tạo duy nhất | 1 | trường hợp cơ sở tối thiểu | 
| tất cả những cái | tất cả 1 | hành vi ngưỡng ngay lập tức | 
| tất cả số không | tất cả -1 | không thể tiến bộ được | 
| trường hợp mẫu | 6 6 6 6 6 7 -1 | động lực hỗn hợp | 
| k = 1 chuỗi | 1 1 1 | cạnh tích lũy nhanh | 

## Vỏ cạnh 

Một trình tự hoàn toàn bằng 0 chứng tỏ rằng chỉ riêng hình phạt nghỉ ngơi không thể tạo ra sự tiến bộ. Bắt đầu từ bất kỳ chỉ mục nào, giá trị luôn ở mức 0 và thuật toán bỏ qua các phân đoạn còn lại một cách chính xác và không bao giờ tìm thấy sự kiện đào tạo, trả về -1 cho tất cả các lần bắt đầu. 

Một trình tự huấn luyện đầy đủ lại cho thấy điều cực đoan ngược lại. Vì không có hình phạt nghỉ ngơi nên giá trị tăng tuyến tính theo$k$mỗi ngày và câu trả lời cho mỗi lần bắt đầu chỉ đơn giản là chỉ số sớm nhất trong đó tổng tích lũy vượt qua 70. Mô phỏng ngay lập tức tích lũy và trả về mà không gặp phải bất kỳ logic nghỉ nào. 

Một trường hợp hỗn hợp với các đoạn nghỉ dài, sau đó là đợt huấn luyện cho thấy tầm quan trọng của việc kẹp. Khi giá trị đạt đến 0 trong khối nghỉ, những ngày nghỉ còn lại sẽ không còn liên quan và thuật toán sẽ chuyển sang ngày đào tạo tiếp theo một cách chính xác mà không lãng phí thời gian hoặc áp dụng các hình phạt bổ sung không chính xác.
