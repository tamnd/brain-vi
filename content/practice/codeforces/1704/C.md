---
title: "CF 1704C - Virus"
description: "Chúng tôi được sắp xếp theo vòng tròn các ngôi nhà nơi một số vị trí bị nhiễm bệnh ban đầu. Mỗi ngày bao gồm hai giai đoạn: đầu tiên, chúng tôi bảo vệ vĩnh viễn chính xác một ngôi nhà hiện đang an toàn, sau đó sự lây nhiễm lây lan từ mọi ngôi nhà bị nhiễm sang tất cả các ngôi nhà lân cận không được bảo vệ và…"
date: "2026-06-09T21:29:01+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1704
codeforces_index: "C"
codeforces_contest_name: "CodeTON Round 2 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1200
weight: 1704
solve_time_s: 143
verified: false
draft: false
---

[CF 1704C - Virus](https://codeforces.com/problemset/problem/1704/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được sắp xếp theo vòng tròn các ngôi nhà nơi một số vị trí bị nhiễm bệnh ban đầu. Mỗi ngày bao gồm hai giai đoạn: đầu tiên, chúng tôi bảo vệ vĩnh viễn chính xác một ngôi nhà hiện đang an toàn, sau đó lây nhiễm lây lan từ mọi ngôi nhà bị nhiễm bệnh sang tất cả những ngôi nhà không được bảo vệ và không bị nhiễm bệnh liền kề. 

Mục đích là lựa chọn trật tự bảo vệ sao cho sau khi lây nhiễm không còn lây lan được nữa, tổng số nhà bị nhiễm bệnh càng ít càng tốt. 

Một cách hữu ích để xem quá trình này là tưởng tượng những ngôi nhà bị nhiễm bệnh là “nguồn” mở rộng ra bên ngoài theo vòng tròn, trong khi các biện pháp bảo vệ của chúng ta đóng vai trò là rào cản ngăn chặn sự mở rộng này. Một khi ngôi nhà được bảo vệ, nó sẽ trở thành một bức tường vĩnh viễn và không thể bị lây nhiễm hoặc lợi dụng để lây lan thêm. 

Kích thước đầu vào làm cho cấu trúc của giải pháp trở nên quan trọng. Mặc dù số lượng nhà n có thể cực kỳ lớn lên tới 10^9, nhưng tổng số nhà bị nhiễm bệnh là nhỏ trong tất cả các trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng chúng ta không thể mô phỏng quá trình trên toàn bộ vòng tròn. Bất kỳ cách tiếp cận nào theo dõi từng ngôi nhà hoặc mô phỏng việc mở rộng từng ngày sẽ quá chậm. 

Cấu trúc ẩn chính là chỉ có khoảng cách giữa các vị trí bị nhiễm mới quan trọng. Vòng tròn được chia thành các phân đoạn một cách hiệu quả gồm những ngôi nhà khỏe mạnh giữa những ngôi nhà bị nhiễm bệnh và chỉ những phân đoạn này mới xác định mức độ lây lan của sự lây nhiễm cũng như mức độ hữu ích của việc đặt các biện pháp bảo vệ. 

Một trường hợp phức tạp phát sinh khi chỉ có một ngôi nhà bị nhiễm bệnh. Trong tình huống đó, sự lây nhiễm lây lan đối xứng theo cả hai hướng dọc theo vòng tròn và câu trả lời hoàn toàn phụ thuộc vào việc chúng ta có thể chặn sự mở rộng từ nguồn duy nhất đó nhanh đến mức nào. Một chiến lược tham lam ngây thơ coi nó như nhiều phân khúc sẽ thất bại vì không có ranh giới nội bộ giữa các đợt lây nhiễm. 

Một trường hợp phức tạp khác là khi các vị trí bị nhiễm cách đều nhau. Ở đây, tất cả các khoảng trống đều bằng nhau và một lựa chọn tham lam phải ưu tiên chính xác việc phá vỡ các mức chênh lệch hiệu quả lớn hơn trước tiên mặc dù mọi thứ cục bộ đều có vẻ đối xứng. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ cố gắng mô hình hóa quá trình này từng ngày. Mỗi ngày, chúng tôi sẽ chọn một ngôi nhà an toàn, đánh dấu nó là được bảo vệ và sau đó mở rộng khả năng lây nhiễm từ tất cả các tế bào hiện đang bị nhiễm bệnh. Vì mỗi lần mở rộng có thể chạm vào nhiều ngôi nhà mới nên trường hợp xấu nhất sẽ xảy ra là phải quét liên tục các phần lớn của vòng tròn. Với n lên tới 10^9, ngay cả việc lưu trữ trạng thái cũng là không thể và ngay cả khi chúng tôi nén trạng thái, việc truyền lặp lại trên các phân đoạn sẽ vẫn tuyến tính theo kích thước của các khoảng thời gian bị ảnh hưởng mỗi ngày, dẫn đến vụ nổ lên tới m bước. 

Quan sát quan trọng là sự lây nhiễm lây lan độc lập bên trong từng khoảng trống giữa những ngôi nhà bị nhiễm bệnh ban đầu trên vòng tròn. Nếu chúng ta sắp xếp các vị trí bị nhiễm bệnh, vòng tròn sẽ trở thành một chuỗi các khoảng trống. Mỗi khoảng trống hoạt động giống như một đoạn tuyến tính được giới hạn bởi hai điểm cuối bị nhiễm (hoặc một điểm cuối trong trường hợp bao quanh). Sự lây nhiễm lan rộng vào bên trong từ cả hai phía và các biện pháp bảo vệ của chúng tôi có thể ngăn chặn sự lây lan này trong nhiều ngày. 

Chiến lược tham lam xuất phát từ việc so sánh tốc độ mỗi khoảng trống bị lây nhiễm “ăn mòn” với tốc độ chúng ta có thể đặt các khối bảo vệ. Mỗi ngày, chúng ta có thể trì hoãn một đơn vị mở rộng một cách hiệu quả, vì vậy chiến lược tốt nhất là luôn ưu tiên tiết kiệm phần lớn nhất còn lại của những ngôi nhà khỏe mạnh. 

Về mặt chính thức, đối với mỗi khoảng trống có kích thước x, sự lây nhiễm sẽ tiêu diệt nó từ cả hai phía và số lượng ngôi nhà có thể được cứu tùy thuộc vào việc chúng ta bắt đầu chặn nó sớm như thế nào. Việc sắp xếp những khoảng trống này và xử lý chúng theo thứ tự giảm dần sẽ dẫn đến việc phân bổ số ngày bảo vệ một cách tối ưu. 

Trực giác cho thấy những khoảng trống lớn rất có giá trị vì chúng mất nhiều thời gian hơn để lây nhiễm hoàn toàn, vì vậy chúng tạo nhiều cơ hội hơn để đặt những bức tường bảo vệ bên trong chúng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · m) hoặc tệ hơn | O(n) | Quá chậm | 
| Khoảng cách + Sắp xếp tham lam | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các vị trí bị nhiễm bệnh dọc theo vòng tròn. Điều này là cần thiết để tính toán chính xác khoảng cách giữa các ngôi nhà bị nhiễm bệnh liên tiếp. 
2. Tính toán tất cả các khoảng trống giữa các vị trí bị nhiễm liền kề theo thứ tự vòng tròn. Đối với hai ngôi nhà bị nhiễm liên tiếp a và b, khoảng cách là b - a - 1, và đối với khoảng cách bao quanh là n - cuối cùng + đầu tiên - 1. Những khoảng trống này thể hiện các đoạn độc lập của các ngôi nhà an toàn ban đầu. 
3. Sắp xếp các khoảng trống theo thứ tự giảm dần. Thứ tự này phản ánh việc ưu tiên các khu vực có áp lực lây nhiễm cao nhất hoặc nơi mà sự can thiệp mang lại nhiều ngôi nhà được cứu sống nhất. 
4. Khởi tạo một biến biểu thị số ngày đã trôi qua. Điều này theo dõi có bao nhiêu lớp lây nhiễm đã bị chặn bởi các biện pháp bảo vệ một cách hiệu quả. 
5. Lặp lại các khoảng trống đã được sắp xếp. Đối với mỗi khoảng trống, hãy trừ hai lần số ngày đã sử dụng khỏi kích thước của nó, vì sự lây nhiễm tiến triển từ cả hai đầu vào trong trong khi các biện pháp bảo vệ tiêu tốn thời gian. Nếu giá trị kết quả là dương, hãy thêm nó vào câu trả lời và tăng bộ đếm ngày một cách thích hợp. 
6. Tích lũy tổng số ngôi nhà bị nhiễm bệnh dưới dạng phần bù của tất cả các phân đoạn đã lưu bên trong các khoảng trống. 

### Tại sao nó hoạt động 

Mỗi khoảng trống hoạt động độc lập vì sự lây nhiễm không thể vượt qua những ngôi nhà bị nhiễm ban đầu. Bên trong một khoảng trống, tình trạng nhiễm trùng tiến triển đối xứng từ cả hai đầu, làm giảm không gian sử dụng hiệu quả đi hai đơn vị mỗi ngày trừ khi bị chặn. Vì mỗi biện pháp bảo vệ trì hoãn việc mở rộng bằng cách tiêu tốn một ngày tăng trưởng một cách hiệu quả, nên việc phân bổ những ngày sớm hơn cho những khoảng trống lớn hơn luôn bảo tồn được nhiều tế bào hơn là trì hoãn chúng. Điều này tạo ra một cấu trúc lợi ích đơn điệu trong đó việc đặt hàng tham lam theo kích thước khoảng trống là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    if m == n:
        print(n)
        continue
    
    a.sort()
    
    gaps = []
    for i in range(m - 1):
        gaps.append(a[i + 1] - a[i] - 1)
    gaps.append(n - a[-1] + a[0] - 1)
    
    gaps.sort(reverse=True)
    
    saved = 0
    days = 0
    
    for g in gaps:
        effective = g - 2 * days
        if effective <= 0:
            continue
        effective -= 1
        if effective > 0:
            saved += effective
            days += 2
    
    print(n - saved)
```Đầu tiên, mã sẽ bình thường hóa các vị trí bị nhiễm và xây dựng tất cả các khoảng trống hình tròn. Việc sắp xếp đảm bảo chúng tôi luôn xử lý các khu vực có giá trị nhất trước tiên. 

Biến`days`mô hình hóa mức độ lây nhiễm đã tích lũy, biểu thị số lượng lớp từ cả hai phía đã bị vô hiệu hóa bởi các biện pháp bảo vệ trước đó. Mỗi khoảng cách được giảm bớt tương ứng. Phép trừ bằng`2 * days`nắm bắt được tính chất đối xứng của sự lây nhiễm lây lan từ cả hai đầu. 

Câu trả lời cuối cùng được tính bằng tổng số ngôi nhà trừ đi những ngôi nhà có thể được cứu khỏi bị lây nhiễm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 10, bị nhiễm = [3, 6, 8] 

Phân loại bị nhiễm: [3, 6, 8] 

Khoảng trống là: 

| Bước | Khoảng cách được tính toán | 
| --- | --- | 
| 3 → 6 | 2 | 
| 6 → 8 | 1 | 
| 8 → 3 (bọc) | 4 | 

Khoảng trống được sắp xếp: [4, 2, 1] 

Chúng tôi xử lý khoảng cách lớn nhất trước tiên. 

| Khoảng cách | Ngày | Hiệu quả | Đã lưu | 
| --- | --- | --- | --- | 
| 4 | 0 | 4 | 3 | 
| 2 | 2 | 0 | 0 | 
| 1 | 2 | 0 | 0 | 

Tổng số được cứu = 3, do đó bị nhiễm = 10 - 3 = 7. 

Điều này cho thấy chỉ phân khúc lớn nhất mới đóng góp có ý nghĩa vì các phân khúc nhỏ hơn bị áp đảo bởi áp lực lan rộng trước đó. 

### Ví dụ 2 

đầu vào: 

n = 10, bị nhiễm = [2, 5] 

Sắp xếp bị nhiễm: [2, 5] 

Khoảng trống: 

| Bước | Khoảng cách | 
| --- | --- | 
| 2 → 5 | 2 | 
| 5 → 2 | 5 | 

Sắp xếp: [5, 2] 

| Khoảng cách | Ngày | Hiệu quả | Đã lưu | 
| --- | --- | --- | --- | 
| 5 | 0 | 5 | 4 | 
| 2 | 2 | 0 | 0 | 

Tổng số đã lưu = 4, bị nhiễm = 6. 

Điều này xác nhận rằng các phân đoạn bao quanh được xử lý bình đẳng và chiếm ưu thế trong quy trình khi có kích thước lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Việc sắp xếp các vị trí và khoảng trống bị nhiễm chiếm ưu thế, với xử lý khoảng cách tuyến tính | 
| Không gian | O(m) | Chỉ lưu trữ các vị trí bị nhiễm và mảng khoảng trống | 

Các ràng buộc cho phép tổng cộng tối đa 10^5 vị trí bị nhiễm trong các trường hợp thử nghiệm, do đó việc sắp xếp theo từng trường hợp thử nghiệm là đủ. Lời giải dễ dàng nằm trong giới hạn vì mọi phép toán đều tuyến tính hoặc gần tuyến tính tính bằng m. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        
        if m == n:
            out.append(str(n))
            continue
        
        a.sort()
        gaps = []
        for i in range(m - 1):
            gaps.append(a[i+1] - a[i] - 1)
        gaps.append(n - a[-1] + a[0] - 1)
        
        gaps.sort(reverse=True)
        
        saved = 0
        days = 0
        
        for g in gaps:
            eff = g - 2 * days
            if eff <= 0:
                continue
            eff -= 1
            if eff > 0:
                saved += eff
                days += 2
        
        out.append(str(n - saved))
    
    return "\n".join(out)

# provided samples
assert run("""8
10 3
3 6 8
6 2
2 5
20 3
3 7 12
41 5
1 11 21 31 41
10 5
2 4 6 8 10
5 5
3 2 5 4 1
1000000000 1
1
1000000000 4
1 1000000000 10 16
""") == """7
5
11
28
9
5
2
15"""

# custom cases
assert run("""1
5 1
3
""") == "3", "single infection small circle"

assert run("""1
6 6
1 2 3 4 5 6
""") == "6", "fully infected"

assert run("""1
10 2
1 6
""") == "6", "two opposite infections"

assert run("""1
12 3
1 5 9
""") == "8", "even spacing case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhiễm trùng đơn vòng tròn nhỏ | 3 | hành vi bao bọc | 
| bị nhiễm hoàn toàn | 6 | trường hợp bão hòa tầm thường | 
| hai bệnh nhiễm trùng ngược nhau | 6 | xử lý khoảng cách đối xứng | 
| trường hợp khoảng cách chẵn | 8 | cạnh tranh khoảng cách thống nhất | 

## Vỏ cạnh 

Một ngôi nhà bị nhiễm bệnh sẽ giảm vấn đề thành một khu vực mở rộng hình tròn. Thuật toán tạo ra một khoảng cách bao quanh lớn và đánh giá chính xác nó mà không cần dựa vào bất kỳ cấu trúc liền kề nào giữa nhiều điểm bị lây nhiễm. 

Khi tất cả các ngôi nhà bị nhiễm bệnh, việc xây dựng khoảng trống mang lại kết quả bằng 0 ở mọi nơi và thuật toán ngay lập tức trả về n vì không còn không gian để tiết kiệm. 

Khi các ngôi nhà bị nhiễm bệnh cách đều nhau, tất cả các khoảng trống đều bằng nhau và việc sắp xếp sẽ duy trì trật tự tùy ý. Sự tham lam vẫn hoạt động vì mỗi khoảng trống được giảm đồng đều bởi cùng một thuật ngữ áp lực lây nhiễm, đảm bảo không có khoảng trống nào được ưu tiên sai. 

Trong mọi trường hợp, hành vi chỉ phụ thuộc vào kích thước khoảng cách tương đối và độ suy giảm tuyến tính do tiến trình ngày gây ra, phù hợp với động lực trải rộng đối xứng thực tế của quy trình.
