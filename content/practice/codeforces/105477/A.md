---
title: "CF 105477A - Tiền xu"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, Baq sở hữu một bộ sưu tập tiền xu, mỗi đồng xu có giá trị nguyên dương. Sử dụng bất kỳ số lượng đồng xu nào, anh ta có thể tính tổng bằng cách chọn một tập hợp con và cộng các giá trị của chúng."
date: "2026-06-23T18:11:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105477
codeforces_index: "A"
codeforces_contest_name: "XXII Spain Olympiad in Informatics, Online Qualifier 1"
rating: 0
weight: 105477
solve_time_s: 68
verified: true
draft: false
---

[CF 105477A - Tiền xu](https://codeforces.com/problemset/problem/105477/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, Baq sở hữu một bộ sưu tập tiền xu, mỗi đồng xu có giá trị nguyên dương. Sử dụng bất kỳ số lượng đồng xu nào, anh ta có thể tính tổng bằng cách chọn một tập hợp con và cộng các giá trị của chúng. Câu hỏi không phải là anh ta có thể tạo ra bao nhiêu tổng, mà là về số nguyên dương nhỏ nhất không thể biểu diễn dưới dạng tổng như vậy. 

Vì vậy, đối với mỗi trường hợp thử nghiệm, chúng tôi đang xem xét một cách hiệu quả tất cả các tập hợp con của một tập hợp các số nguyên dương và yêu cầu khoảng cách đầu tiên trong tập hợp các tổng có thể đạt được. 

Hạn chế chính là số lượng xu có thể lớn, lên tới 10.000 trong những trường hợp khó nhất và giá trị đồng xu có thể lớn tới 10^9. Điều này ngay lập tức loại trừ bất kỳ nỗ lực nào nhằm liệt kê rõ ràng các tổng tập hợp con, vì ngay cả đối với n vừa phải, số lượng tổng tập hợp con vẫn tăng theo cấp số nhân. Ngay cả việc lập trình động trên tổng cũng không khả thi vì phạm vi tổng là không giới hạn và có thể cực kỳ lớn. 

Một trường hợp thất bại tinh tế đối với lối suy nghĩ ngây thơ là giả định rằng chúng ta cần tạo ra tất cả các tổng có thể đạt được và sau đó quét tìm số tiền còn thiếu đầu tiên. Ví dụ: với đồng xu [1, 3], tổng có thể đạt được là {1, 3, 4}, do đó thiếu 2. Nhưng nếu chúng ta cố gắng ép buộc tất cả các kết hợp, chúng ta đã vượt quá khả năng tính toán khả thi đối với các đầu vào lớn hơn một chút như 30 đồng xu tùy ý. 

Một trực giác sai lầm khác là chỉ cần phân loại là đủ. Việc sắp xếp giúp cấu trúc lại lý luận, nhưng nếu không có logic tích lũy tham lam chính xác thì sẽ không trực tiếp mang lại câu trả lời. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tạo mọi tổng tập hợp con và sau đó kiểm tra xem số nguyên dương nào bị thiếu. Điều này hoạt động về mặt khái niệm vì nó trực tiếp mô hình hóa định nghĩa về khả năng tiếp cận. Tuy nhiên, mỗi đồng xu nhân đôi số lượng tập hợp con, vì vậy với n đồng xu, chúng ta nhận được tổng 2^n. Với n = 40, con số này đã trở nên lớn về mặt thiên văn và hạn chế cho phép lên tới 10.000 xu, khiến phương pháp này về cơ bản là không thể. 

Quan sát quan trọng xuất phát từ quan điểm thay đổi: thay vì theo dõi tất cả các tổng, chúng tôi theo dõi tiền tố nhỏ nhất của các số nguyên dương mà chúng tôi có thể hình thành liên tục bắt đầu từ 1. Giả sử chúng tôi đã xác định rằng mọi giá trị từ 1 đến x đều có thể được hình thành. Bây giờ hãy xem xét một giá trị đồng xu mới v. Nếu v lớn hơn x + 1, thì x + 1 ngay lập tức không thể truy cập được, bởi vì tất cả các tổng hiện có nhiều nhất là x và việc thêm v sẽ vượt qua khoảng trống mà không lấp đầy nó. Nếu v tối đa là x + 1 thì chúng ta có thể mở rộng khoảng có thể đạt tới x + v, bởi vì mọi giá trị lên đến x đều có thể được kết hợp với v để lấp đầy phân đoạn tiếp theo. 

Việc mở rộng khoảng tham lam này là ý tưởng cốt lõi. Thay vì suy nghĩ về mặt kết hợp, chúng tôi duy trì một phân đoạn ngày càng tăng của số tiền có thể biểu diễn được. Khoảng trống đầu tiên xuất hiện chính xác khi một đồng xu đến mà không thể bù đắp cho giá trị còn thiếu tiếp theo. 

Việc sắp xếp các đồng xu là điều cần thiết vì trước tiên chúng tôi muốn xử lý các giá trị nhỏ để xây dựng phạm vi liên tục nhỏ nhất một cách hiệu quả nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tổng tập hợp con) | O(2^n) | O(2^n) | Quá chậm | 
| Mở rộng khoảng tham lam | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp giá trị đồng xu theo thứ tự không giảm. Điều này đảm bảo chúng tôi luôn cố gắng mở rộng phạm vi tiếp cận nhỏ nhất trước tiên. 
2. Khởi tạo một biến`reach = 0`, nghĩa là hiện tại chúng ta có thể tạo tất cả các tổng từ 1 đến`reach`liên tục. 
3. Lặp lại qua từng giá trị đồng xu`v`theo thứ tự sắp xếp. 
4. Nếu`v > reach + 1`, dừng ngay và xuất`reach + 1`. Đây là giá trị đầu tiên không thể hình thành được vì chúng ta có một khoảng cách không thể bắc cầu. 
5. Nếu không, hãy cập nhật`reach = reach + v`. Điều này mở rộng phạm vi có thể tiếp cận liên tục. 
6. Sau khi xử lý tất cả các đồng tiền, xuất ra`reach + 1`, vì chúng ta có thể hình thành mọi thứ lên đến`reach`nhưng không có gì hơn thế. 

Lý do chính ở bước 4 là nếu có khoảng trống ở`reach + 1`, không có đồng tiền nào sau này có thể sửa được vì tất cả các đồng tiền sau này ít nhất phải lớn bằng`v`, do đó cũng quá lớn để điền vào giá trị còn thiếu. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào sau khi xử lý một số tiền tố của các đồng tiền được sắp xếp, chúng tôi duy trì tính bất biến mà mọi số nguyên trong phạm vi`[1, reach]`có thể biểu diễn bằng cách sử dụng một số tập hợp con của các đồng tiền được xử lý. Khi chúng ta nhìn thấy một đồng xu`v`như vậy`v <= reach + 1`, chúng ta có thể kết hợp nó với tất cả các khoản tiền có thể đạt được trước đó để mở rộng phạm vi có thể tiếp cận mà không tạo ra khoảng trống. Nếu như`v > reach + 1`, không có cách nào để xây dựng`reach + 1`, vì tất cả các kết hợp đều nằm trong`[1, reach]`hoặc nhảy vượt ra ngoài nó. Bất biến này đảm bảo rằng lần vi phạm đầu tiên tương ứng chính xác với số tiền nhỏ nhất không thể tiếp cận được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    arr = list(map(int, input().split()))
    arr.sort()

    reach = 0
    for v in arr:
        if v > reach + 1:
            break
        reach += v

    print(reach + 1)
```Giải pháp bắt đầu bằng cách sắp xếp các đồng xu để chúng tôi luôn xem xét chúng theo thứ tự tăng dần, điều này cần thiết để duy trì khoảng thời gian có thể tiếp cận liền kề. Biến`reach`theo dõi tổng liên tục tối đa mà chúng ta hiện có thể hình thành. Mỗi đồng xu sẽ kéo dài khoảng thời gian này hoặc để lộ một khoảng trống. điều kiện`v > reach + 1`nắm bắt chính xác số nguyên không thể truy cập đầu tiên, vì`reach + 1`là giá trị còn thiếu nhỏ nhất ở bất kỳ giai đoạn nào. 

Một lỗi triển khai phổ biến là khởi tạo`reach`không chính xác. Nó phải bắt đầu từ 0 vì trước khi sử dụng bất kỳ đồng xu nào, chỉ có thể đạt được tổng trống và tổng dương đầu tiên mà chúng ta quan tâm là 1. Một điểm tinh tế khác là bất đẳng thức nghiêm ngặt: sử dụng`>=`thay vì`>`phá vỡ tính đúng đắn, vì một đồng xu bằng`reach + 1`vẫn nên mở rộng phạm vi có thể truy cập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[2, 4, 16, 8, 1]`Sau khi sắp xếp:`[1, 2, 4, 8, 16]`| Bước | Đồng xu v | đạt trước | Tình trạng | tiếp cận sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 1 | 1 | 
| 2 | 2 | 1 | 2 2 2 | 3 | 
| 3 | 4 | 3 | 4 4 4 | 7 | 
| 4 | 8 | 7 | 8 8 8 | 15 | 
| 5 | 16 | 15 | 16 16 | 31 | 

Sau khi xử lý tất cả tiền xu, phạm vi có thể tiếp cận là`[1, 31]`, vậy đáp án là 32. 

Dấu vết này cho thấy sức mạnh của sự tăng trưởng giống như hai tiếp tục kéo dài một khoảng thời gian hoàn toàn liên tục mà không có khoảng trống. 

### Ví dụ 2 

đầu vào:`[3, 5, 7, 1]`Đã sắp xếp:`[1, 3, 5, 7]`| Bước | Đồng xu v | đạt trước | Tình trạng | tiếp cận sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 1 | 1 | 
| 2 | 3 | 1 | 3 > 2 | dừng lại | 

Chúng ta dừng ngay ở đồng xu thứ hai vì không thể tạo thành số 2. Đáp án là 2. 

Điều này cho thấy một cây cầu bị thiếu sớm theo thứ tự đã sắp xếp sẽ ngay lập tức xác định kết quả như thế nào, bất kể đồng xu sau này như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét tuyến tính sau đó | 
| Không gian | O(1) | ngoài bộ nhớ đầu vào, chỉ có một số biến được sử dụng | 

Các ràng buộc cho phép tối đa 10.000 xu cho mỗi thử nghiệm và tối đa 500 trường hợp thử nghiệm, do đó, tệ nhất là khoảng 5 triệu phần tử được xử lý trong các hoạt động sắp xếp tổng thể. Điều này phù hợp thoải mái trong giới hạn trong Python với khả năng xử lý đầu vào hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        arr.sort()

        reach = 0
        for v in arr:
            if v > reach + 1:
                break
            reach += v

        out.append(str(reach + 1))

    return "\n".join(out)

# provided samples
assert run("""3
5
2 4 16 8 1
3
1 2 3
4
3 5 7 1
""") == """32
7
2"""

# minimum size
assert run("""1
1
1
""") == "2"

# gap immediately
assert run("""1
2
2 3
""") == "1"

# full continuous
assert run("""1
4
1 2 3 4
""") == "11"

# large powers of two behavior
assert run("""1
5
1 2 4 8 16
""") == "32"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồng xu duy nhất 1 | 2 | trường hợp biên nhỏ nhất | 
| 2,3 | 1 | khoảng cách ngay lập tức khi bắt đầu | 
| 1 2 3 4 | 11 | mở rộng hoàn toàn liên tục | 
| sức mạnh của hai | 32 | mô hình tăng trưởng liền kề tối đa | 

## Vỏ cạnh 

Đối với một đồng xu có giá trị 1, mảng được sắp xếp là`[1]`. Bộ thuật toán`reach = 1`và vì không có khoảng trống nào xuất hiện nên nó xuất ra`2`. Điều này phù hợp với thực tế là chỉ có 1 có thể xây dựng được và thiếu 2. 

Đối với những đồng tiền như`[2, 3]`, sắp xếp mang lại thứ tự tương tự. Ban đầu`reach = 0`, và đồng xu đầu tiên là 2. Vì`2 > 1`, thuật toán dừng ngay lập tức và trả về`1`, xác định chính xác rằng không thể hình thành tổng dương. 

Đối với một bộ hoàn toàn dày đặc như`[1, 2, 3, 4]`, mỗi đồng xu thỏa mãn`v <= reach + 1`, do đó khoảng có thể truy cập tăng liên tục lên 10 và câu trả lời trở thành 11. Điều này xác nhận bất biến rằng không có khoảng trống nào xuất hiện khi mỗi đồng xu mới đủ nhỏ để bắc cầu cho số nguyên còn thiếu tiếp theo.
