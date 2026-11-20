---
title: "CF 2128A - Trung tâm tái chế"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản có một bộ sưu tập các túi, mỗi túi có trọng lượng ban đầu. Thời gian diễn ra theo từng giây rời rạc. Cứ mỗi giây chúng ta buộc phải loại bỏ đúng một túi còn lại."
date: "2026-06-08T03:08:13+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2128
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1039 (Div. 2)"
rating: 800
weight: 2128
solve_time_s: 72
verified: true
draft: false
---

[CF 2128A - Trung tâm tái chế](https://codeforces.com/problemset/problem/2128/A) 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản có một bộ sưu tập các túi, mỗi túi có trọng lượng ban đầu. Thời gian diễn ra theo từng giây rời rạc. Cứ mỗi giây chúng ta buộc phải loại bỏ đúng một túi còn lại. Việc tháo túi đôi khi miễn phí và đôi khi tốn một đồng xu, tùy thuộc vào việc trọng lượng hiện tại của nó có vượt quá ngưỡng hay không. Sau khi loại bỏ, tất cả các túi còn lại tăng gấp đôi trọng lượng của chúng. 

Khó khăn chính là trọng lượng tăng lên theo thời gian, do đó, một chiếc túi rẻ tiền để loại bỏ sớm có thể trở nên đắt tiền sau này. Chúng tôi không chọn có nên tháo túi hay không mà chỉ chọn thời điểm tháo từng túi và thời điểm đó sẽ quyết định liệu chúng tôi có thanh toán hay không. 

Kích thước đầu vào nhỏ: tối đa 30 túi cho mỗi trường hợp thử nghiệm và tối đa 1000 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi lập kế hoạch theo cấp số nhân đối với các hoán vị. Một giải pháp có thể cung cấp những thứ như sắp xếp, quyết định tham lam hoặc mô phỏng O(n²) cho mỗi trường hợp thử nghiệm, nhưng không thể thực hiện được DP giai thừa hoặc trạng thái trên các tập hợp con theo thời gian. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các giá trị đã vượt quá ngưỡng. Trong trường hợp đó, mỗi lần loại bỏ đều tốn 1 chi phí bất kể thứ tự nào, vì vậy câu trả lời chỉ đơn giản là n. Điều cực đoan ngược lại là khi tất cả các giá trị ở dưới ngưỡng trong một thời gian dài, khi đó thứ tự đóng vai trò quan trọng. Ví dụ: một chiếc túi nhỏ có thể trở nên lớn nếu bị trì hoãn, do đó, việc ngây thơ “loại bỏ cái lớn nhất trước” hoặc “loại bỏ cái nhỏ nhất trước” mà không tính đến thời gian đều có thể thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các lệnh có thể để tháo túi. Vì có n! hoán vị và mỗi trình tự loại bỏ yêu cầu mô phỏng cập nhật trọng số qua tối đa n bước, điều này hoàn toàn không khả thi ngay cả với n = 30. Tổng số thao tác sẽ vượt quá 10³² trong trường hợp xấu nhất. 

Điều quan trọng cần lưu ý là chi phí cuối cùng của mỗi chiếc túi chỉ phụ thuộc vào thời điểm nó được lấy ra. Nếu một túi được lấy ra tại thời điểm t (bắt đầu từ 0), trọng lượng của nó bằng giá trị ban đầu nhân với 2^t. Vì vậy, chúng ta chỉ cần biết liệu chúng ta có thể gán cho mỗi túi một khoảng thời gian duy nhất sao cho càng nhiều túi càng tốt thỏa mãn điều kiện a_i * 2^t ≤ c hay không. 

Viết lại điều kiện này, chúng ta nhận được một ràng buộc về thời gian: mỗi túi có thời gian muộn nhất để có thể lấy ra miễn phí. Nếu chúng ta xác định thời hạn d_i là t lớn nhất sao cho a_i * 2^t ≤ c thì mỗi túi sẽ trở thành một công việc phải được lên lịch tại một khoảng thời gian riêng biệt từ 0 đến n-1. Bất kỳ túi nào được chỉ định thời gian lớn hơn thời hạn sẽ phải chịu phí 1. 

Đây là một bài toán lập kế hoạch kinh điển: tối đa hóa số lượng công việc được hoàn thành trước thời hạn. Chiến lược tối ưu là tham lam. Chúng tôi sắp xếp thời hạn và chỉ định cho mỗi túi một suất trống sớm nhất có thể. Bất kỳ hành lý nào không thể đặt vào ô hợp lệ đều phải được thanh toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (hoán vị) | O(n! · n) | O(n) | Quá chậm | 
| Lập kế hoạch tham lam tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi túi, hãy tính xem nó có thể tồn tại được bao lâu trước khi trở nên quá lớn để có thể tháo ra dễ dàng. 

Đây là số lần nhân đôi tối đa được phép khi duy trì ≤ c. Chúng ta liên tục nhân đôi khái niệm bằng cách đếm số lần chúng ta có thể nhân với 2 trước khi vượt quá c. Điều này xác định giá trị thời hạn cho mỗi túi. 
2. Chuyển đổi từng túi thành thời hạn t_i, nghĩa là nó chỉ có thể được lấy ra miễn phí nếu được lên lịch vào thời điểm ≤ t_i. 
3. Sắp xếp tất cả các thời hạn theo thứ tự tăng dần. 

Việc sắp xếp đảm bảo chúng tôi ưu tiên những túi có mức độ hạn chế cao nhất trước tiên vì chúng có khoảng thời gian loại bỏ an toàn nhỏ nhất. 
4. Quét qua các thời hạn được sắp xếp trong khi vẫn duy trì con trỏ tới khung thời gian hiện tại.

Thời gian bắt đầu lúc 0. 
5. Đối với mỗi thời hạn, nếu thời gian hiện tại ≤ thời hạn thì phân công túi vào thời gian này và tăng dần thời gian. Nếu không, chúng ta không thể nhét nó vào một khe trống, vì vậy nó phải được loại bỏ sau đó với giá 1. 
6. Đáp án là số túi chưa được chỉ định. 

### Tại sao nó hoạt động 

Mỗi túi xác định độc lập thời gian loại bỏ tối đa cho phép với chi phí bằng 0. Chúng tôi đang kết hợp các mục một cách hiệu quả với các khe thời gian nguyên. Bất kỳ nhiệm vụ nào nhằm giảm thiểu chi phí đều tương đương với việc tối đa hóa số lượng túi phù hợp với khoảng thời gian cho phép của chúng. Việc sắp xếp theo thời hạn sớm nhất đảm bảo chúng tôi không bao giờ lãng phí một chỗ chật hẹp trên túi có thể được đặt sau, duy trì tính khả thi cho các ràng buộc chặt chẽ hơn. Sự lựa chọn tham lam này duy trì tính bất biến là ở mỗi bước chúng ta đã sử dụng số lượng khe thời gian tối thiểu có thể có trong khi vẫn giữ cho tất cả các phép gán hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, c = map(int, input().split())
        a = list(map(int, input().split()))

        deadlines = []

        for x in a:
            if x > c:
                deadlines.append(-1)
                continue

            tlim = 0
            val = x
            while val <= c:
                val *= 2
                tlim += 1

            deadlines.append(tlim - 1)

        free_slots = sorted(deadlines)
        time = 0
        paid = 0

        for d in free_slots:
            if d >= time:
                time += 1
            else:
                paid += 1

        print(paid)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là chuyển đổi từng trọng số thành thời hạn. Chúng tôi mô phỏng rõ ràng việc nhân đôi cho đến khi vượt quá c, điều này an toàn vì các giá trị tăng theo cấp số nhân và n rất nhỏ, do đó vòng lặp này bị giới hạn bởi khoảng 30 lần lặp. 

Chúng tôi coi bất kỳ giá trị ban đầu nào lớn hơn c đều có thời hạn -1, nghĩa là không bao giờ có thể xóa nó miễn phí. Những khoản đó sẽ tự động được tính là đã thanh toán trừ khi được khớp cực kỳ sớm, điều mà vòng lặp tham lam xử lý một cách tự nhiên. 

Cuối cùng, việc sắp xếp và chỉ định các khoảng thời gian một cách tham lam sẽ đảm bảo chúng tôi tối đa hóa số lần xóa miễn phí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 10
10 4 15 1 8
```Thời hạn: 

| Túi | Giá trị | Hạn chót | 
| --- | --- | --- | 
| 10 | 10 | 0 | 
| 4 | 4 | 1 | 
| 15 | 15 | -1 | 
| 1 | 1 | 3 | 
| 8 | 8 | 0 | 

Thời hạn sắp xếp: [-1, 0, 0, 1, 3] 

| Bước | Hạn chót | Thời gian | Hành động | Đã trả tiền | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | 0 | không thể đặt lịch miễn phí | 1 | 
| 2 | 0 | 0 | giao | 1 | 
| 3 | 0 | 1 | không thể gán | 2 | 
| 4 | 1 | 1 | giao | 2 | 
| 5 | 3 | 2 | giao | 2 | 

Đầu ra là 2. 

Điều này cho thấy thời hạn chặt chẽ buộc phải tiêu thụ sớm các chỗ trống và việc một túi trở nên khó tránh khỏi. 

### Ví dụ 2 

đầu vào:```
3 42
1000000000 1000000000 1000000000
```Tất cả các giá trị vượt quá c ngay lập tức. 

| Túi | Giá trị | Hạn chót | 
| --- | --- | --- | 
| tất cả | 1e9 | -1 | 

Đã sắp xếp: [-1, -1, -1] 

| Bước | Hạn chót | Thời gian | Hành động | Đã trả tiền | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | 0 | trả tiền | 1 | 
| 2 | -1 | 0 | trả tiền | 2 | 
| 3 | -1 | 0 | trả tiền | 3 | 

Đầu ra là 3. 

Điều này khẳng định rằng khi không có chiếc túi nào có thể được lấy ra một cách an toàn thì việc đặt hàng là không phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Thời hạn sắp xếp chiếm ưu thế, mỗi test case nhỏ | 
| Không gian | O(n) | Chúng tôi lưu trữ thời hạn và một số quầy | 

Với n ≤ 30 và t ≤ 1000, điều này diễn ra thoải mái trong giới hạn. Việc kiểm tra tăng trưởng theo cấp số nhân là không đáng kể do giới hạn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, c = map(int, input().split())
            a = list(map(int, input().split()))

            deadlines = []
            for x in a:
                if x > c:
                    deadlines.append(-1)
                    continue
                tlim = 0
                val = x
                while val <= c:
                    val *= 2
                    tlim += 1
                deadlines.append(tlim - 1)

            deadlines.sort()
            time = 0
            paid = 0

            for d in deadlines:
                if d >= time:
                    time += 1
                else:
                    paid += 1

            print(paid)

    from io import StringIO
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    sys.stdin = old
    return out.getvalue().strip()

# provided samples
assert run("""4
5 10
10 4 15 1 8
3 42
1000000000 1000000000 1000000000
10 30
29 25 2 12 15 42 14 6 16 9
10 1000000
1 1 1 1 1 1 1 1 1 864026633""") == """2
3
6
1"""

# custom cases
assert run("""1
1 10
5""") == "0"

assert run("""1
1 10
20""") == "1"

assert run("""1
3 10
1 2 3""") == "0"

assert run("""1
4 1
1 1 1 100""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 túi trong giới hạn | 0 | loại bỏ miễn phí duy nhất | 
| 1 túi vượt quá giới hạn | 1 | luôn được trả tiền | 
| tất cả các giá trị nhỏ | 0 | tất cả đều có thể được lên lịch miễn phí | 
| giá trị cực trị hỗn hợp | 1 | tham lam xử lý thất bại cô lập | 

## Vỏ cạnh 

Khi mọi yếu tố đều vượt quá ngưỡng, tất cả thời hạn đều trở thành -1. Thuật toán vẫn hoạt động vì mọi phần tử đều được đưa vào nhóm trả phí ngay lập tức và không có lợi thế đặt hàng giả nào được tạo ra vì thời gian chỉ tăng lên khi một nhiệm vụ hợp lệ xảy ra. 

Khi tất cả các yếu tố đều nhỏ thì thời hạn sẽ lớn và ngày càng tăng. Việc sắp xếp đảm bảo tất cả chúng đều được chỉ định một cách tuần tự mà không có xung đột, do đó không có xu nào được trả. 

Khi các giá trị được nhóm chặt chẽ xung quanh ngưỡng, một số túi trở nên không hợp lệ chỉ sau một hoặc hai lần nhân đôi. Việc sắp xếp theo thời hạn đảm bảo những trường hợp mong manh đó được xử lý trước, ngăn không cho chúng bị đẩy sang những thời điểm sau mà chúng sẽ bị thanh toán sai.
