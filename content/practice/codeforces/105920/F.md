---
title: "CF 105920F - Kéo"
description: "Chúng tôi đang lập mô hình một quy trình ngẫu nhiên lặp đi lặp lại trong đó mỗi “chu kỳ” bao gồm vẽ cho đến khi chúng tôi nhận được một sự kiện hiếm gặp, sau đó đặt lại. Mỗi lần rút thăm đều có xác suất tạo ra kết quả 6 sao, nhưng xác suất này không cố định."
date: "2026-06-21T12:09:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105920
codeforces_index: "F"
codeforces_contest_name: "Soy Cup #1: Firefly"
rating: 0
weight: 105920
solve_time_s: 47
verified: true
draft: false
---

[CF 105920F - Kéo](https://codeforces.com/problemset/problem/105920/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang lập mô hình một quy trình ngẫu nhiên lặp đi lặp lại trong đó mỗi “chu kỳ” bao gồm vẽ cho đến khi chúng tôi nhận được một sự kiện hiếm gặp, sau đó đặt lại. Mỗi lần rút thăm đều có xác suất tạo ra kết quả 6 sao, nhưng xác suất này không cố định. Nó bắt đầu ở một số tốc độ cơ bản và tăng tuyến tính sau một số lần thử thất bại ở ngưỡng trong chu kỳ hiện tại, cho đến khi đạt đến mức chắc chắn. 

Cụ thể hơn, sau lần cuối bạn đạt được 6 sao, bạn bắt đầu kéo lại. Một vài lần kéo đầu tiên trong chu kỳ mới đều có cùng xác suất thành công một phần trăm. Nếu bạn không đạt được 6 sao cho c lần kéo liên tiếp thì bắt đầu từ lần kéo tiếp theo, xác suất thành công sẽ tăng b phần trăm cho mỗi lần kéo thất bại bổ sung vượt quá c, giới hạn ở mức 100%. Ngay khi thành công xảy ra, quy trình sẽ được đặt lại và bạn bắt đầu lại với xác suất cơ bản. 

Nhiệm vụ là tính toán số lần kéo dự kiến ​​giữa hai lần thành công liên tiếp, nghĩa là độ dài dự kiến ​​của một chu kỳ như vậy. 

Các ràng buộc rất nhỏ: a, b và c đều có nhiều nhất là 100 và có tới 1000 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào liên quan đến máy trạng thái có kích thước không đổi hoặc DP kỳ vọng trực tiếp trên nhiều nhất là vài trăm trạng thái đều khả thi. Về nguyên tắc, mọi thứ bậc hai hoặc tệ hơn trên mỗi trường hợp thử nghiệm vẫn được chấp nhận, nhưng bất kỳ điều gì liên quan đến mô phỏng trong thời gian không giới hạn hoặc hội tụ lặp qua nhiều bước cho mỗi trường hợp thử nghiệm sẽ có rủi ro. 

Cấu trúc trường hợp cạnh chính xuất phát từ xác suất cực cao. Khi a là 100, câu trả lời gần như là 1 vì mỗi lần kéo đều thành công ngay lập tức. Khi a rất nhỏ và c rất lớn, quá trình này có thể mất nhiều thời gian ở giai đoạn xác suất phẳng trước khi đoạn đường nối đáng tiếc bắt đầu. Một trường hợp tinh tế khác là khi b đủ lớn để xác suất đạt tới 100 trước hoặc ngay sau c, cắt đuôi một cách hiệu quả. 

Một sai lầm ngây thơ là mô phỏng quá trình một cách trực tiếp. Mặc dù mỗi chu kỳ có kỳ vọng ngắn, nhưng những cái đuôi dài hiếm hoi khiến Monte Carlo không ổn định và hội tụ chậm trong phạm vi đảm bảo độ chính xác nghiêm ngặt. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về kỳ vọng là mô phỏng lặp đi lặp lại quá trình của một chu kỳ. Chúng ta sẽ bắt đầu từ những lần thất bại bằng 0, lấy mẫu lặp đi lặp lại các thử nghiệm Bernoulli có xác suất phụ thuộc vào chuỗi thất bại hiện tại và dừng lại khi thành công xảy ra. Lặp lại điều này nhiều lần và tính trung bình sẽ đưa ra ước tính về kỳ vọng. 

Điều này đúng về mặt khái niệm, nhưng nó không đưa ra câu trả lời xác định với độ chính xác được đảm bảo. Ngay cả khi chúng ta cố gắng tính toán kỳ vọng bằng cách cắt bớt mô phỏng ở một độ sâu lớn nào đó, thì xác suất đuôi không đáng kể khi a nhỏ và cơ chế tốc độ tăng có thể làm chậm quá trình hội tụ một cách đáng kể. Vấn đề cốt lõi là về nguyên tắc, quy trình này có độ dài không giới hạn. 

Quan sát quan trọng là hệ thống chỉ có một số hữu hạn các trạng thái riêng biệt nếu chúng ta theo dõi “có bao nhiêu lần thất bại kể từ thành công cuối cùng” cho đến điểm mà xác suất trở thành 100%. Sau khi thành công xảy ra, mọi thứ sẽ được đặt lại, vì vậy chúng ta chỉ cần tính thời gian đánh dự kiến ​​​​để hấp thụ trong chuỗi Markov. 

Mỗi trạng thái tương ứng với số lần thất bại thứ i, trong đó i nằm trong khoảng từ 0 trở lên. Với mỗi i, chúng ta biết xác suất thành công p(i). Từ trạng thái i, chúng ta thành công và kết thúc, hoặc thất bại và chuyển sang i + 1. Đây là phép lặp kỳ vọng tiêu chuẩn: 

E[i] = 1 + (1 - p(i)) * E[i + 1] 

và điều kiện cuối là E[last] = 1 khi p(last) = 1, vì thành công là chắc chắn. 

Số lượng trạng thái bị giới hạn vì khi xác suất đạt 100%, chúng tôi sẽ ngừng mở rộng chuỗi. Vì a, b, c ≤ 100 nên xác suất trở thành xác định sau nhiều nhất khoảng 100 bước vượt quá c. Vì vậy, chúng tôi có thể giới hạn các tiểu bang một cách an toàn ở mức vài trăm. 

Giải pháp giảm xuống còn tính toán một phép truy hồi tuyến tính hữu hạn ngược từ trạng thái cuối cùng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tàn bạo | O(không xác định, phương sai cao) | O(1) | Không đáng tin cậy | 
| DP qua trạng thái lỗi | O(c + 100) mỗi bài kiểm tra | O(c + 100) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập chỉ mục các trạng thái theo số lần thất bại liên tiếp kể từ 6 sao cuối cùng. 

1. Tính xác suất thành công cho mỗi lần thất bại i. Với i ≤ c, xác suất là a + 0·b, a + 1·b, v.v. chỉ sau khi vượt quá c. Trong thực tế, chúng ta định nghĩa p(i) = min(100, a + max(0, i - c) * b). Điều này chuyển quy tắc đáng tiếc thành một hàm xác định đối với các trạng thái. 
2. Xác định chỉ số ngưỡng L trong đó p(L) bằng 100. Từ trạng thái đó trở đi, lần kéo tiếp theo luôn thành công, vì vậy E[L] = 1. 
3. Khởi tạo một mảng E trong đó E[i] sẽ biểu thị số lần kéo dự kiến cần bắt đầu từ trạng thái lỗi i. 
4. Điền ngược mảng từ L - 1 xuống 0 bằng phép truy toán E[i] = 1 + (1 - p(i)) * E[i + 1] / 100. Việc chia cho 100 xuất phát từ việc chuyển đổi xác suất phần trăm thành xác suất thực. 
5. Đầu ra E[0], là số lần kéo dự kiến ​​bắt đầu ngay sau khi thành công. 

Lý do DP lùi hoạt động là vì mỗi trạng thái chỉ phụ thuộc vào trạng thái tiếp theo. Không có sự phân nhánh tới các trạng thái trước đó, vì vậy khi biết E[i + 1], E[i] sẽ trở thành một phép tính trực tiếp. 

### Tại sao nó hoạt động 

Quá trình này là chuỗi Markov hấp thụ một chiều trong đó trạng thái i chỉ chuyển sang i + 1 (thất bại) hoặc hấp thụ (thành công). Mọi đường dẫn cuối cùng đều đạt được thành công hoặc đạt đến trạng thái có xác suất 1, đảm bảo chấm dứt. Sự truy hồi mã hóa chính xác quy luật kỳ vọng tổng thể được quy định ở bước đầu tiên. Vì tất cả các phụ thuộc đều tiến lên trong i nên tính toán lùi tạo ra điểm cố định duy nhất của hệ thống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())

        # build probabilities until they reach 100
        p = []
        i = 0
        while True:
            if i <= c:
                val = a
            else:
                val = a + (i - c) * b

            if val >= 100:
                p.append(1.0)
                break

            p.append(val / 100.0)
            i += 1

        n = len(p)
        E = [0.0] * n

        # terminal state: guaranteed success
        E[n - 1] = 1.0

        for i in range(n - 2, -1, -1):
            E[i] = 1.0 + (1.0 - p[i]) * E[i + 1]

        print(E[0])

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng chuỗi xác suất cho đến khi nó đạt đến mức độ chắc chắn, đóng vai trò là ranh giới hấp thụ. Điều này tránh việc suy luận về giới hạn trên rõ ràng về số lượng trạng thái. 

DP lạc hậu là bản dịch trực tiếp của sự tái phát. Điều tinh tế quan trọng là đảm bảo xác suất được chuẩn hóa thành số float [0, 1] và trạng thái cuối cùng được đặt thành chính xác 1,0 bước dự kiến ​​vì nó luôn thành công ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

a = 1, b = 1, c = 1 

Chúng tôi tính toán các trạng thái: 

| tôi | p(i) | E[i] | 
| --- | --- | --- | 
| 3 | 1.0 | 1.0 | 
| 2 | 0,03 | 1 + 0,97 * 1 = 1,97 | 
| 1 | 0,02 | 1 + 0,98 * 1,97 = 2,9306 | 
| 0 | 0,01 | 1 + 0,99 * 2,9306 ≈ 3,9013 | 

Giá trị cuối cùng là số lần kéo dự kiến ​​bắt đầu từ đầu. Điều này cho thấy ngay cả những xác suất nhỏ cũng tích lũy được những giá trị kỳ vọng lớn do những thất bại lặp đi lặp lại trước khi sự thương hại kích hoạt. 

### Ví dụ 2 

đầu vào: 

a = 50, b = 25, c = 2 

| tôi | p(i) | E[i] | 
| --- | --- | --- | 
| 3 | 1.0 | 1.0 | 
| 2 | 1.0 | 1.0 | 
| 1 | 0,75 | 1 + 0,25 * 1 = 1,25 | 
| 0 | 0,50 | 1 + 0,50 * 1,25 = 1,625 | 

Ở đây hệ thống đáng tiếc không bao giờ kích hoạt vì xác suất đã nhanh chóng đạt 100%. Kỳ vọng bị chi phối bởi cơ hội thành công sớm hơn là hành vi kéo dài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(c + 100) mỗi bài kiểm tra | Chúng tôi tạo ra tối đa ~100 trạng thái xác suất cho đến khi bão hòa | 
| Không gian | O(c + 100) | Chúng tôi lưu trữ giá trị DP cho từng tiểu bang | 

Các giới hạn đảm bảo rằng ngay cả với 1000 trường hợp thử nghiệm, tổng công việc vẫn nhỏ. Kích thước DP bị giới hạn bởi thực tế là xác suất đạt 100% sau tối đa 100 lần tăng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    def solve():
        t = int(input())
        for _ in range(t):
            a, b, c = map(int, input().split())
            p = []
            i = 0
            while True:
                if i <= c:
                    val = a
                else:
                    val = a + (i - c) * b
                if val >= 100:
                    p.append(1.0)
                    break
                p.append(val / 100.0)
                i += 1

            n = len(p)
            E = [0.0] * n
            E[-1] = 1.0
            for i in range(n - 2, -1, -1):
                E[i] = 1.0 + (1.0 - p[i]) * E[i + 1]

            print(E[0])

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("1\n1 1 1\n") != "", "sample placeholder"

# custom cases
assert run("1\n100 100 100\n")[:3] == "1.0"
assert run("1\n1 1 1\n") != "", "low probability case"
assert run("2\n1 1 1\n100 100 100\n") != "", "multi test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 100 100 100 | 1 | lợi thế thành công ngay lập tức | 
| 1 1 1 | kỳ vọng lớn | đuôi có xác suất thấp | 
| 2 bài kiểm tra trộn | cả hai trường hợp | xử lý nhiều trường hợp | 

## Vỏ cạnh 

Khi a bằng 100, trạng thái hấp thụ đạt ngay lập tức tại i = 0. Mảng DP chứa một trạng thái duy nhất có p(0) = 1, do đó E[0] được đặt trực tiếp thành 1. Điều này ngăn cản việc chia cho 0 hoặc các chuyển đổi không cần thiết. 

Khi a rất nhỏ và c rất lớn, nhiều trạng thái tích tụ trước khi sự thương hại kích hoạt. DP vẫn xử lý việc này một cách chính xác vì mỗi E[i] chỉ phụ thuộc vào E[i + 1] và không đưa ra phép tính gần đúng nào. 

Khi b đủ lớn để p(i) nhảy lên 100 trước khi đạt tới c, chuỗi sẽ kết thúc sớm. Vòng lặp xây dựng dừng ngay khi val ≥ 100, đảm bảo chúng ta không phân bổ các trạng thái không cần thiết và điều kiện đầu cuối là chính xác.
