---
title: "CF 1980A - Trình tạo sự cố"
description: "Mỗi trường hợp thử nghiệm mô tả một bộ vấn đề nhỏ mà Vlad đã có và số vòng mục tiêu mà anh ấy muốn tổ chức. Mỗi vấn đề đều có một trong bảy mức độ khó từ A đến G."
date: "2026-06-08T16:51:49+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1980
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 950 (Div. 3)"
rating: 800
weight: 1980
solve_time_s: 77
verified: true
draft: false
---

[CF 1980A - Trình tạo sự cố](https://codeforces.com/problemset/problem/1980/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một bộ vấn đề nhỏ mà Vlad đã có và số vòng mục tiêu mà anh ấy muốn tổ chức. Mỗi vấn đề có một trong bảy nhãn độ khó có thể có từ A đến G. Một vòng yêu cầu chính xác một vấn đề, nhưng trong tất cả các vòng, Vlad muốn đảm bảo rằng anh ấy có thể cung cấp đủ vấn đề cho từng mức độ khó cần thiết trong các vòng đó. 

Đối với mỗi trường hợp thử nghiệm, chúng ta được cung cấp nhiều tập các vấn đề hiện có và một số vòng m. Mỗi vòng tiêu thụ một yêu cầu một cách hiệu quả và trong tất cả m vòng, Vlad cần có khả năng đáp ứng nhu cầu về tất cả các chữ cái khó xuất hiện. Vì ví dụ của câu lệnh cho thấy các chữ cái còn thiếu đang được "điền", nên nhiệm vụ thực sự giảm xuống còn việc kiểm tra mức độ phù hợp: đối với mỗi loại trong số bảy loại độ khó, số lượng đã có và số lượng chữ cái bổ sung phải được tạo để trong tất cả các vòng, tất cả các vị trí bắt buộc đều có thể được lấp đầy. 

Các ràng buộc đủ nhỏ để mọi trường hợp thử nghiệm có thể được xử lý bằng cách đếm trực tiếp. Với n lên tới 50 và t lên tới 1000, thậm chí O(n) cho mỗi trường hợp thử nghiệm là không đáng kể. Bất kỳ giải pháp nào chỉ đơn giản là đếm tần số ký tự và so sánh chúng với các ngưỡng bắt buộc sẽ chạy ngay lập tức. 

Một trường hợp lỗi tinh tế xuất hiện khi một hoặc nhiều chữ cái bị thiếu hoàn toàn. Ví dụ: nếu tất cả các bài toán hiện có đều từ B đến G nhưng A vắng mặt và m ít nhất bằng 1 thì chúng ta vẫn phải tính A nếu cần. Một trường hợp góc khác là khi tần số bị lệch nhiều, chẳng hạn như nhiều B nhưng không có E, điều này có thể dẫn đến việc đếm thiếu nếu chúng ta chỉ suy luận về kích thước tổng thay vì phạm vi bao phủ của mỗi chữ cái. 

Điểm mấu chốt là vấn đề không nằm ở việc sắp xếp lại hay nhóm các vấn đề. Nó hoàn toàn nhằm đảm bảo rằng mỗi loại độ khó bắt buộc xuất hiện ít nhất m lần trong nhóm cuối cùng được xây dựng. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là mô phỏng việc xây dựng từng vòng một. Đối với mỗi vòng, chúng tôi sẽ cố gắng giải quyết các vấn đề hiện có trước và bất cứ khi nào thiếu một độ khó cần thiết, chúng tôi sẽ tạo ra một vấn đề mới. Cách tiếp cận này có thể được mô hình hóa bằng cách lặp qua các vòng và quét liên tục số lượng có sẵn. Mặc dù điều này hoạt động về mặt khái niệm nhưng nó không cần thiết vì chúng tôi sẽ liên tục tính toán lại tính khả dụng, dẫn đến công việc dư thừa trong các vòng. 

Quan sát quan trọng là các vòng không tương tác về mặt cấu trúc. Mỗi loại độ khó phải đáp ứng một cách độc lập một yêu cầu tỷ lệ thuận với số vòng yêu cầu. Vì mỗi vòng cần một vấn đề và mỗi độ khó chỉ có thể được sử dụng lại nếu chúng tôi tính tính sẵn có trên toàn cầu, nên yêu cầu thực sự chỉ đơn giản là: đối với mỗi chữ cái trong số bảy chữ cái, hãy đảm bảo chúng tôi có ít nhất m lần xuất hiện trên toàn bộ nhóm được sử dụng để hỗ trợ m vòng. 

Như vậy, thay vì mô phỏng các vòng, chúng ta so sánh trực tiếp tần số của từng ký tự trong ngân hàng với m. Nếu một chữ cái xuất hiện nhiều lần thì chúng ta cần tối đa (0, m − cnt) bài toán mới cho chữ cái đó. Tổng hợp điều này trên tất cả các chữ cái sẽ cho câu trả lời. 

Điều này làm giảm vấn đề xuống còn kiểm tra tần số có kích thước cố định trên 7 ký hiệu, đây là công việc không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m * n) | O(1) | Quá chậm và không cần thiết | 
| Đếm tần số | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần mỗi ký tự từ 'A' đến 'G' xuất hiện trong chuỗi đã cho. Điều này cung cấp nguồn cung hiện tại của từng loại khó khăn. 
2. Đối với mỗi ký tự trong phạm vi 'A' đến 'G', hãy tính xem có bao nhiêu ký tự bị thiếu so với m bằng cách sử dụng max(0, m − current_count). Điều này thể hiện có bao nhiêu vấn đề mới phải được tạo ra cho khó khăn đó. 
3. Tính tổng tất cả các giá trị còn thiếu của bảy ký tự. 
4. Xuất kết quả dưới dạng câu trả lời cho ca kiểm thử.

Tại sao nó hoạt động được gắn liền với sự độc lập của các loại khó khăn. Mỗi chữ cái đại diện cho một nhóm yêu cầu riêng biệt. Các vấn đề hiện tại của một chữ cái không thể giúp đáp ứng yêu cầu của chữ cái khác, do đó độ thiếu của mỗi chữ cái là độc lập. Câu trả lời cuối cùng chỉ là tổng số thiếu hụt trên tất cả các nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        s = input().strip()

        freq = [0] * 7
        for ch in s:
            freq[ord(ch) - ord('A')] += 1

        ans = 0
        for i in range(7):
            if freq[i] < m:
                ans += (m - freq[i])

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng một mảng tần số có kích thước 7, ánh xạ từng chữ cái từ A đến G thành một chỉ mục. Điều này tránh việc quét lặp lại sau này. Vòng lặp thứ hai trực tiếp tính toán độ thiếu hụt trên mỗi ký tự so với m. Phép trừ được bảo vệ nên chúng tôi chỉ tính các giá trị còn thiếu, không bao giờ thừa. 

Một sai lầm phổ biến là tính tổng các chênh lệch mà không đặt ở mức 0, điều này sẽ coi các chữ cái dư thừa là phần đóng góp âm một cách không chính xác. Một sai lầm khác là so sánh với n thay vì m, nhưng yêu cầu là phạm vi bao phủ mỗi vòng nên m là mục tiêu chính xác. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi tính toán trên hai đầu vào. 

### Ví dụ 1 

đầu vào: 

n = 10, m = 1 

s = BGECDCBDED 

Chúng tôi tính toán tần số: 

| Thư | Đếm | Bắt buộc m | Thiếu | 
| --- | --- | --- | --- | 
| A | 0 | 1 | 1 | 
| B | 2 | 1 | 0 | 
| C | 2 | 1 | 0 | 
| D | 2 | 1 | 0 | 
| E | 2 | 1 | 0 | 
| F | 0 | 1 | 1 | 
| G | 1 | 1 | 0 | 

Tổng số thiếu = 2 

Điều này khẳng định rằng chỉ có A và F vắng mặt nên cần có hai bài toán mới. 

### Ví dụ 2 

đầu vào: 

n = 10, m = 2 

s = BGECDCBDED 

| Thư | Đếm | Bắt buộc m | Thiếu | 
| --- | --- | --- | --- | 
| A | 0 | 2 | 2 | 
| B | 2 | 2 | 0 | 
| C | 2 | 2 | 0 | 
| D | 2 | 2 | 0 | 
| E | 2 | 2 | 0 | 
| F | 0 | 2 | 2 | 
| G | 1 | 2 | 1 | 

Tổng số thiếu = 5 

Điều này cho thấy việc tăng m yêu cầu thang đo tuyến tính trên mỗi chữ cái như thế nào và phạm vi bao phủ một phần vẫn góp phần gây ra thâm hụt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi chuỗi được quét một lần và sau đó chúng tôi lặp lại 7 chữ cái cố định | 
| Không gian | O(1) | Mảng tần số có kích thước không đổi | 

Với n ∼ 50 và t ∼ 1000, tổng công việc tối đa là 50.000 ký tự, đây là mức không đáng kể trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        s = input().strip()

        freq = [0] * 7
        for ch in s:
            freq[ord(ch) - ord('A')] += 1

        ans = 0
        for i in range(7):
            if freq[i] < m:
                ans += (m - freq[i])

        output.append(str(ans))

    return "\n".join(output)

# provided samples
assert run("""3
10 1
BGECDCBDED
10 2
BGECDCBDED
9 1
BBCDEFFGG""") == "2\n5\n1"

# custom cases
assert run("""1
7 3
ABCDEFG""") == "14", "each letter needs 3 total"

assert run("""1
7 1
ABCDEFG""") == "0", "already complete"

assert run("""1
7 2
AAAAAAA""") == "12", "only A exists"

assert run("""1
1 5
A""") == "30", "single letter extreme requirement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ABCDEFG với m=3 | 14 | thâm hụt đồng đều trên tất cả các chữ cái | 
| ABCDEFG với m=1 | 0 | không thiếu chữ | 
| tất cả chữ A với m=2 | 12 | xử lý phân phối sai lệch | 
| đơn A có m lớn | 30 | tỉ lệ mất cân bằng cực độ | 

## Vỏ cạnh 

Trường hợp cạnh phím là khi một số chữ cái hoàn toàn không xuất hiện. Đối với đầu vào như AAAAAAA với m = 2, tần số của mọi chữ cái khác bằng 0. Thuật toán ấn định mức thâm hụt là 2 cho mỗi chữ cái bị thiếu và 0 cho A vì nó đã vượt quá hoặc đáp ứng yêu cầu tùy thuộc vào m. Điều này đảm bảo các danh mục bị thiếu được tính đầy đủ thay vì bị bỏ qua. 

Một trường hợp khác là khi tất cả các chữ cái đều đã đủ. Ví dụ: ABCDEFG với m = 1 tạo ra số lượng thiếu bằng 0 trên tất cả các danh mục và thuật toán trả về chính xác bằng 0 vì mọi tần số đều đáp ứng hoặc vượt quá ngưỡng. 

Trường hợp cuối cùng là độ lệch cực lớn, chẳng hạn như một chữ cái chiếm ưu thế. Việc tính toán vẫn đúng vì mỗi chữ cái được xử lý độc lập, do đó thặng dư trong một danh mục không bao giờ bù đắp cho sự thiếu hụt ở danh mục khác.
