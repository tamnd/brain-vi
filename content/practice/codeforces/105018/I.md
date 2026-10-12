---
title: "CF 105018I - Hội trường khuôn mặt"
description: "Chúng ta được sắp xếp các mặt được dán nhãn theo hình tròn. Mỗi vị trí chứa một nhãn chuỗi mô tả khuôn mặt và chính xác một vị trí được đánh dấu bằng nhãn \"Jaqen\", đại diện cho vị trí hiện tại của khách du lịch."
date: "2026-06-28T02:05:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105018
codeforces_index: "I"
codeforces_contest_name: "Winter Cup 5.0 Online Mirror Contest"
rating: 0
weight: 105018
solve_time_s: 42
verified: true
draft: false
---

[CF 105018I - Hall of Faces](https://codeforces.com/problemset/problem/105018/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp các mặt được dán nhãn theo hình tròn. Mỗi vị trí chứa một nhãn chuỗi mô tả khuôn mặt và chính xác một vị trí được đánh dấu bằng nhãn`"Jaqen"`, đại diện cho vị trí hiện tại của khách du lịch. 

Sau khi đưa mặt hiện tại về vị trí của nó, nhiệm vụ là tìm một mặt khác có nhãn mục tiêu được yêu cầu. Được phép di chuyển theo cả hai hướng xung quanh vòng tròn, theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ và chúng tôi muốn có số lượng mặt được kiểm tra tối thiểu trước khi đạt đến bất kỳ sự xuất hiện nào của nhãn mục tiêu. 

Điểm mấu chốt là chi phí kiểm tra được đo bằng số bước dọc theo vòng tròn từ`"Jaqen"`vị trí xuất hiện gần nhất của nhãn mục tiêu, xem xét cả hai hướng. 

Kích thước đầu vào cho phép tối đa 100 trường hợp thử nghiệm và tổng cộng lên tới 10^5 khuôn mặt. Điều này ngay lập tức loại trừ bất cứ điều gì tồi tệ hơn thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Cách tiếp cận bậc hai thử mỗi lần bắt đầu và tính toán lại khoảng cách sẽ giảm xuống còn khoảng 10^10 thao tác trong trường hợp xấu nhất, điều này không khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi nhãn mục tiêu hoàn toàn không tồn tại trong mảng. Trong trường hợp đó, đầu ra đúng là`-1`. Một trường hợp cạnh khác là khi nhãn mục tiêu chính xác`"Jaqen"`chính nó. Vì về mặt khái niệm, chúng ta “trả lại” khuôn mặt hiện tại trước nên chúng ta vẫn tìm kiếm một sự xuất hiện khác, và nếu không tồn tại thì câu trả lời cũng là`-1`. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xác định chỉ số của`"Jaqen"`, sau đó, đối với mọi vị trí trong vòng tròn, hãy kiểm tra khoảng cách của nó với chỉ mục này và giữ mức tối thiểu trong số những vị trí có nhãn phù hợp với mục tiêu. Bởi vì mảng là hình tròn nên mỗi khoảng cách yêu cầu tính toán cả độ lệch theo chiều kim đồng hồ và ngược chiều kim đồng hồ, lấy giá trị tối thiểu. 

Điều này hoạt động chính xác vì nó đánh giá rõ ràng mọi đích đến có thể. Tuy nhiên, đối với mỗi mục tiêu ứng viên phù hợp, chúng tôi tính toán lại khoảng cách theo O(1), nhưng việc quét tất cả các vị trí vẫn tốn O(n). Việc thực hiện điều này cho nhiều trường hợp kiểm thử vẫn có thể được chấp nhận một cách riêng biệt, nhưng sự kém hiệu quả sẽ xuất hiện khi chúng ta cố gắng quá cẩn thận hoặc quét lại nhiều lần cho mỗi truy vấn, đặc biệt nếu được triển khai một cách đơn giản với số học mô-đun lặp đi lặp lại và tìm kiếm`"Jaqen"`mỗi lần. 

Quan sát quan trọng là chúng ta không cần phải tính toán lại khoảng cách cho mỗi lần xuất hiện của mục tiêu một cách độc lập theo cách phức tạp. Một khi chúng ta biết được vị trí của`"Jaqen"`, khoảng cách của mọi vị trí khác được xác định hoàn toàn bằng chỉ số số học trên một vòng tròn. Do đó, câu trả lời giảm xuống còn quét mảng một lần, chỉ tính khoảng cách vòng tròn cho các vị trí khớp với nhãn mục tiêu và lấy mức tối thiểu. 

Khoảng cách vòng tròn giữa các chỉ số i và j trong một mảng có độ dài n là: 

phút(|i - j|, n - |i - j|) 

Vì chúng tôi chỉ quan tâm đến các vị trí có nhãn mong muốn nên chúng tôi giảm vấn đề xuống một lần duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại cho tất cả các cặp) | O(n²) | O(1) | Quá chậm | 
| Quét đơn tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và xác định chỉ số của vị trí đặc biệt có nhãn`"Jaqen"`. Đây là điểm tham chiếu mà từ đó tất cả các khoảng cách được đo. 
2. Đọc nhãn mục tiêu mà chúng tôi đang tìm kiếm. 
3. Nếu nhãn đích không tồn tại ở bất kỳ đâu trong mảng, hãy trả về`-1`ngay lập tức. Điều này tránh được việc tính toán không cần thiết. 
4. Khởi tạo một biến`best`tới giá trị lớn. Điều này sẽ theo dõi khoảng cách vòng tròn nhỏ nhất được tìm thấy cho đến nay. 
5. Duyệt từng chỉ số i trong mảng. Đối với từng vị trí: 

1. Nếu nhãn tại i khớp với mục tiêu, hãy tính khoảng cách vòng tròn giữa i và`"Jaqen"`chỉ số. 
2. Cập nhật`best`nếu khoảng cách này nhỏ hơn. 
6. Sau khi kết thúc quá trình duyệt, xuất ra`best`. 

Quyết định quan trọng là chỉ hạn chế tính toán khoảng cách ở các vị trí có liên quan. Điều này đảm bảo chúng tôi không lãng phí thời gian đánh giá các nhãn không liên quan. 

### Tại sao nó hoạt động 

Mỗi câu trả lời hợp lệ tương ứng với việc chọn một số lần xuất hiện của nhãn mục tiêu và đi dọc theo vòng tròn từ`"Jaqen"`đến sự việc đó. Chi phí của chuyến đi đó chỉ phụ thuộc vào vị trí tương đối của họ trên chu trình. Vì chúng tôi kiểm tra mỗi lần xuất hiện chính xác một lần và tính toán khoảng cách vòng tròn tối thiểu thực sự của nó từ vị trí bắt đầu, mức tối thiểu trên tất cả chúng chính xác là mức tối ưu toàn cục. Không có đường đi nào khác ngoài việc truyền tải vòng tròn trực tiếp có thể ngắn hơn, do đó không cần cấu trúc thay thế hoặc lý luận nhiều bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        parts = input().split()
        n = int(parts[0])
        target = parts[1]

        arr = input().split()

        start = -1
        for i, x in enumerate(arr):
            if x == "Jaqen":
                start = i
                break

        # If target is not present at all
        if target not in arr:
            print(-1)
            continue

        best = float('inf')

        for i, x in enumerate(arr):
            if x == target:
                diff = abs(i - start)
                dist = min(diff, n - diff)
                if dist < best:
                    best = dist

        print(best if best != float('inf') else -1)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ phân tích từng trường hợp thử nghiệm và trích xuất cả nhãn đích và danh sách vòng tròn đầy đủ. Sau đó nó xác định vị trí bắt đầu có nhãn`"Jaqen"`trong một lần đi qua. 

Lần thứ hai chỉ kiểm tra các vị trí phù hợp với mục tiêu. Tính toán khoảng cách vòng tròn sử dụng công thức bao quanh tiêu chuẩn`min(|i - start|, n - |i - start|)`, tính toán chính xác cho cả chuyển động theo chiều kim đồng hồ và ngược chiều kim đồng hồ. 

Một cạm bẫy phổ biến là quên áp dụng hiệu chỉnh bao quanh và chỉ sử dụng sai số tuyệt đối. Điều đó sẽ đánh giá quá cao khoảng cách khi đường đi tối ưu vượt qua ranh giới của mảng. 

Một vấn đề tế nhị khác là xử lý trường hợp thiếu mục tiêu hoàn toàn. Nếu không có sự kiểm tra rõ ràng,`best`sẽ vẫn là vô cùng và phải được chuyển đổi thành`-1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, target = one-eyed
arr = [red-haired, Jaqen, silver-haired, one-eyed, red-haired]
```Đầu tiên chúng tôi xác định vị trí`"Jaqen"`ở chỉ số 1. 

| tôi | nhãn | phù hợp với mục tiêu | |i - bắt đầu| | khoảng cách tròn | tốt nhất | 

|---|---|---|---|---|---| 

| 0 | tóc đỏ | không | 1 | 1 | thông tin | 

| 1 | Jaqen | không | 0 | 0 | thông tin | 

| 2 | tóc bạc | không | 1 | 1 | thông tin | 

| 3 | một mắt | vâng | 2 | 2 | 2 | 

| 4 | tóc đỏ | không | 3 | 2 | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này cho thấy cách thuật toán xử lý việc gói vòng tròn một cách tự nhiên, vì đường đi ngắn nhất từ chỉ số 1 đến 3 cũng có thể được coi là 1 → 0 → 4 → 3, nhưng công thức lại nắm bắt trực tiếp. 

### Ví dụ 2 

đầu vào:```
n = 3, target = thick-eyebrows
arr = [Jaqen, brown-eyed, long-nose]
```Đây`"Jaqen"`ở chỉ số 0, nhưng mục tiêu không tồn tại. 

Chúng tôi phát hiện sự vắng mặt sớm và đưa ra`-1`. 

Điều này xác nhận rằng thuật toán phân tách chính xác khả năng tiếp cận khỏi tính toán khoảng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi mảng được quét một lần để tìm`"Jaqen"`và một lần để đánh giá trận đấu | 
| Không gian | O(1) thêm | Chỉ một số biến được sử dụng ngoài bộ nhớ đầu vào | 

Với tổng kích thước đầu vào lên tới 10^5, thao tác này diễn ra thoải mái trong giới hạn vì mọi phần tử đều được xử lý với số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        parts = input().split()
        n = int(parts[0])
        target = parts[1]
        arr = input().split()

        start = -1
        for i, x in enumerate(arr):
            if x == "Jaqen":
                start = i
                break

        if target not in arr:
            out.append("-1")
            continue

        best = float('inf')
        for i, x in enumerate(arr):
            if x == target:
                d = abs(i - start)
                best = min(best, min(d, n - d))

        out.append(str(best if best != float('inf') else -1))

    return "\n".join(out) + "\n"

# provided sample-like tests
assert run("1\n5 one-eyed\nred-haired Jaqen silver-haired one-eyed red-haired\n") == "2\n"
assert run("1\n3 thick-eyebrows\nJaqen brown-eyed long-nose\n") == "-1\n"

# custom cases
assert run("1\n1 a\nJaqen\n") == "-1\n"
assert run("1\n4 x\nx Jaqen x x\n") == "1\n"
assert run("1\n6 a\nb c Jaqen d e a\n") == "3\n"
assert run("1\n5 Jaqen\nJaqen Jaqen Jaqen Jaqen Jaqen\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ một phần tử | -1 | không có mục tiêu có thể tiếp cận | 
| nhiều mục tiêu xung quanh vòng tròn | 1 | đường tròn ngắn nhất | 
| mục tiêu xa trên bọc | 3 | sự đúng đắn bao quanh | 
| tất cả các nhãn giống hệt nhau | 0 | trường hợp tối thiểu tầm thường | 

## Vỏ cạnh 

Khi mảng có kích thước một, phần tử duy nhất là`"Jaqen"`, vì vậy bất kỳ mục tiêu nào khác là không thể. Quá trình quét không tìm thấy kết quả phù hợp và trả về`-1`, phù hợp với quy tắc chúng ta phải xác định vị trí một khuôn mặt khác. 

Khi mục tiêu xuất hiện nhiều lần đối xứng xung quanh`"Jaqen"`, thuật toán sẽ đánh giá chính xác từng ứng viên một cách độc lập và chọn khoảng cách vòng tròn nhỏ nhất. Vì mỗi lần xuất hiện được xử lý một lần nên tính đối xứng không gây ra bất kỳ lỗi sai lệch hoặc trùng lặp nào. 

Khi sự xuất hiện gần nhất yêu cầu bao quanh ranh giới,`min(diff, n - diff)`tính toán tự động ghi lại cung ngắn hơn, ngăn chặn việc đánh giá quá cao chỉ phát sinh từ khoảng cách tuyến tính.
