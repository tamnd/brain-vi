---
title: "CF 105637B - Lễ Hội Hoa"
description: "Bài toán mô tả một con đường thẳng có chiều dài (f) dẫn đến địa điểm lễ hội tại vị trí (f). Có (n) ô tô hiện đang chạy trên con đường này. Mỗi ô tô có một vị trí đã biết (xi), được đo bằng khoảng cách từ điểm bắt đầu của con đường và tốc độ không đổi (vi)."
date: "2026-06-26T14:21:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105637
codeforces_index: "B"
codeforces_contest_name: "The 2022 ICPC Asia Tehran Regional Contest"
rating: 0
weight: 105637
solve_time_s: 42
verified: true
draft: false
---

[CF 105637B - Lễ hội hoa](https://codeforces.com/problemset/problem/105637/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một con đường thẳng có chiều dài\(f\)dẫn đến một địa điểm lễ hội tại vị trí\(f\). có\(n\)ô tô hiện đang lưu thông trên con đường này. Mỗi chiếc xe đều có một vị trí đã biết\(x_i\), được đo bằng khoảng cách từ điểm bắt đầu của con đường và tốc độ không đổi\(v_i\). Tất cả các ô tô đều di chuyển về phía lễ hội nên vị trí của chúng tăng tuyến tính theo thời gian cho đến khi chúng đạt được\(f\). 

Nhiệm vụ là xác định chiếc xe nào đến lễ hội trước. Vì mỗi ô tô chuyển động với vận tốc không đổi nên thời gian ô tô đến\(i\)chỉ đơn giản là khoảng cách còn lại đến điểm cuối chia cho tốc độ của nó, \((f - x_i) / v_i\). Đáp án là chỉ số của xe có thời gian nhỏ nhất như vậy. Đảm bảo rằng không có hai ô tô nào đến đích cùng một lúc nên có một đáp án duy nhất. 

Các hạn chế là nhỏ:\(n \leq 100\),\(f \leq 10^4\)và tốc độ cũng bị giới hạn bởi\(100\). Điều này có nghĩa là ngay cả việc quét \(O(n)\) đơn giản với số học cơ bản cũng là quá đủ. Bất kỳ giải pháp nào tính toán mỗi thời gian đến một lần và so sánh chúng trực tiếp sẽ chạy ngay lập tức. Không cần phải sắp xếp hoặc cấu trúc dữ liệu nâng cao vì đầu ra chỉ ở mức tối thiểu trên một tập hợp nhỏ. 

Một số trường hợp đặc biệt cần được nêu rõ. Nếu ô tô đã ở vị trí\(f\), thì thời gian đến của nó bằng 0, và nó phải được chọn nếu không có xe nào khác cũng ở mức 0 (điều mà vấn đề đảm bảo sẽ không xảy ra). Nếu nhiều xe xuất phát ở cùng một vị trí nhưng có tốc độ khác nhau thì rõ ràng xe nào nhanh hơn sẽ thắng vì quãng đường còn lại là như nhau. Một sai lầm tinh vi sẽ phát sinh nếu người ta so sánh\(x_i / v_i\)thay vì \((f - x_i) / v_i\), sẽ tính toán thời gian di chuyển từ đầu thay vì thời gian đến đích, đảo ngược hoàn toàn logic. 

Một cạm bẫy dễ mắc phải khác là sử dụng phép chia dấu phẩy động và so sánh thời gian trực tiếp mà không cần quan tâm. Mặc dù điều này hoạt động trong điều kiện hạn chế nhưng nó không cần thiết và kém hiệu quả hơn so với việc so sánh các sản phẩm chéo để tránh các vấn đề về độ chính xác. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để giải quyết vấn đề là mô phỏng kết quả cuộc đua cuối cùng bằng số. Đối với mỗi ô tô, hãy tính xem sẽ mất bao lâu để đến điểm cuối, sau đó chọn mức tối thiểu. Điều này đúng vì chuyển động là đồng đều và độc lập giữa các ô tô nên thời gian đến hoàn toàn quyết định thứ tự. 

Chế độ xem bạo lực đã tối ưu trong cài đặt này. Ngay cả khi chúng ta tưởng tượng việc tính toán lại các vị trí ở những bước thời gian nhỏ, điều đó có nghĩa là phải cập nhật liên tục tất cả\(n\)ô tô qua một số lượng lớn các bước theo thời gian\(f\), dẫn đến cái gì đó giống như \(O(n \cdot f)\), điều này là không cần thiết vì chuyển động là tuyến tính và có thể được biểu diễn ở dạng khép kín. 

Quan sát quan trọng là trạng thái của mỗi ô tô được tóm tắt đầy đủ bằng một giá trị vô hướng duy nhất: thời gian còn lại để đạt được\(f\). Khi chúng tôi tính toán điều đó một lần cho mỗi ô tô, vấn đề sẽ giảm xuống việc tìm mức tối thiểu trên\(n\)các giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Mô phỏng tàn bạo theo các bước thời gian | \(O(n \cdot f)\) | \(O(1)\) | Quá chậm | 
| Tính toán thời gian trực tiếp | \(O(n)\) | \(O(1)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc\(n\)Và\(f\), xác định có bao nhiêu ô tô tồn tại và vị trí của vạch đích. 

2. Khởi tạo các biến để theo dõi thời gian đến tốt nhất (nhỏ nhất) được thấy cho đến nay và chỉ số xe tương ứng. Điều quan trọng là phải khởi tạo thời điểm tốt nhất cho thứ gì đó lớn hơn bất kỳ giá trị thực nào có thể có, để chiếc ô tô đầu tiên luôn trở thành đường cơ sở. 

3. Đối với mỗi xe\(i\), đọc vị trí của nó\(x_i\)và tốc độ\(v_i\). 

4. Tính quãng đường còn lại tới lễ hội như sau\(d_i = f - x_i\). Điều này thể hiện quãng đường mà chiếc xe vẫn cần phải đi. 

5. Tính thời gian đến một cách khái niệm như\(t_i = d_i / v_i\). Thay vì thực sự chia, chúng tôi so sánh bằng cách sử dụng phép nhân chéo để tránh các vấn đề về độ chính xác: chúng tôi so sánh\(d_i \cdot v_{\text{best}}\)với\(d_{\text{best}} \cdot v_i\). 

6. Nếu ô tô hiện tại đến sớm hơn ô tô nổi tiếng nhất, hãy cập nhật biểu thị thời gian tốt nhất và lưu trữ chỉ số của ô tô này. 

7. Sau khi xử lý tất cả các xe, xuất ra chỉ số của xe có thời gian đến tối thiểu. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì tính bất biến rằng ô tô được lưu trữ là ô tô có thời gian đến nhỏ nhất trong số tất cả các ô tô được xử lý. Mỗi chiếc xe mới được so sánh với ứng cử viên này bằng cách sử dụng thứ tự phân số tương đương về mặt toán học. Vì thời gian đến hoàn toàn có thể so sánh được và không tồn tại mối ràng buộc nào nên tính bất biến đảm bảo rằng sau khi quét tất cả các ô tô, chỉ số được lưu trữ tương ứng với mức tối thiểu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def better(d1, v1, d2, v2):
    return d1 * v2 < d2 * v1

n, f = map(int, input().split())

best_idx = -1
best_d = 0
best_v = 1

for i in range(1, n + 1):
    x, v = map(int, input().split())
    d = f - x

    if best_idx == -1:
        best_idx = i
        best_d, best_v = d, v
    else:
        if better(d, v, best_d, best_v):
            best_idx = i
            best_d, best_v = d, v

print(best_idx)
```Mã này duy trì ứng cử viên tốt nhất hiện tại dưới dạng một cặp \((d, v)\) thay vì lưu trữ thời gian một cách rõ ràng. Điều này tránh số học dấu phẩy động và giữ cho phép so sánh chính xác bằng cách sử dụng phép nhân chéo. Hàm so sánh trợ giúp mã hóa thứ tự\(d_1 / v_1 < d_2 / v_2\)trong số học số nguyên. 

Việc khởi tạo với`best_idx = -1`đảm bảo chiếc xe đầu tiên luôn được chọn làm đường cơ sở. Mỗi chiếc xe tiếp theo được so sánh với nó và các cập nhật chỉ xảy ra khi tìm thấy thời gian đến nhỏ hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 200
0 1
10 5
40 1
```Chúng tôi theo dõi khoảng cách còn lại và lựa chọn tốt nhất: 

| tôi | x | v | d = 200 - x | kết quả so sánh | chỉ số tốt nhất | 
|---|---|---|---|---|---| 
| 1 | 0 | 1 | 200 | chiếc xe đầu tiên | 1 | 
| 2 | 10 | 5 | 190 | 190/5 < 200/1 | 2 | 
| 3 | 40 | 1 | 160 | 160/1 > 190/5 | 2 | 

Xe 2 thắng vì đi được quãng đường còn lại nhanh nhất dù ban đầu không phải là xe gần nhất. 

### Ví dụ 2 

đầu vào:```
5 100
0 1
10 3
60 2
75 1
10 4
```| tôi | x | v | d | kết quả so sánh | tốt nhất | 
|---|---|---|---|---|---| 
| 1 | 0 | 1 | 100 | đầu tiên | 1 | 
| 2 | 10 | 3 | 90 | 90/3 < 100/1 | 2 | 
| 3 | 60 | 2 | 40 | 40/2 > 90/3 | 2 | 
| 4 | 75 | 1 | 25 | 25/1 > 90/3 | 2 | 
| 5 | 10 | 4 | 90 | 90/4 < 90/3 | 5 | 

Câu trả lời cuối cùng là xe 5 vì nó cải thiện ở xe 2 nhờ tốc độ cao hơn, mặc dù vị trí giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(n)\) | Mỗi ô tô được xử lý một lần với số học theo thời gian không đổi | 
| Không gian | \(O(1)\) | Chỉ một số biến vô hướng được lưu trữ | 

Với\(n \leq 100\), giải pháp này thấp hơn nhiều so với mọi ràng buộc về giới hạn thời gian. Ngay cả trong cài đặt chặt chẽ hơn, quá trình quét tuyến tính vẫn ở mức tối ưu vì mọi yếu tố đầu vào phải được kiểm tra ít nhất một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, f = map(int, sys.stdin.readline().split())

    best_idx = -1
    best_d = 0
    best_v = 1

    def better(d1, v1, d2, v2):
        return d1 * v2 < d2 * v1

    for i in range(1, n + 1):
        x, v = map(int, sys.stdin.readline().split())
        d = f - x

        if best_idx == -1:
            best_idx = i
            best_d, best_v = d, v
        else:
            if better(d, v, best_d, best_v):
                best_idx = i
                best_d, best_v = d, v

    return str(best_idx)

# provided samples
assert run("""3 200
0 1
10 5
40 1
""") == "2"

assert run("""5 100
0 1
10 3
60 2
75 1
10 4
""") == "5"

# custom cases
assert run("""1 100
50 10
""") == "1"

assert run("""2 100
0 10
90 1
""") == "1"

assert run("""3 100
10 1
10 2
10 3
""") == "3"

assert run("""4 100
0 1
50 2
70 5
90 10
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| xe đơn | 1 | trường hợp ranh giới tối thiểu | 
| xe xa chậm vs nhanh | 1 | tốc độ chiếm ưu thế khoảng cách | 
| cùng một vị trí tốc độ khác nhau | 3 | hòa qua tốc độ | 
| tăng khoảng cách nhưng tốc độ chậm | 4 | xếp hạng không đơn điệu | 

## Vỏ cạnh 

Đầu vào của một chiếc ô tô là không đáng kể vì không có sự so sánh nào; thuật toán khởi tạo chính xác chiếc ô tô đầu tiên là chiếc tốt nhất và ngay lập tức xuất ra nó. 

Khi một chiếc ô tô xuất phát rất gần đích đến nhưng cực kỳ chậm và một chiếc ô tô khác xuất phát xa hơn nhưng nhanh hơn, thuật toán sẽ so sánh chính xác các tỷ lệ đầy đủ thay vì khoảng cách thô. Ví dụ, với\(f = 100\), ô tô \((x=90, v=1)\) và \((x=0, v=10)\), thời gian được tính toán là\(10\)Và\(10\), nhưng bài toán đảm bảo không có ràng buộc, nên bất đẳng thức nghiêm ngặt quyết định một cách nhất quán. 

Khi nhiều ô tô ở cùng một vị trí, việc so sánh chỉ giảm về tốc độ vì khoảng cách còn lại của chúng giống nhau. Quy tắc nhân chéo tự nhiên đơn giản hóa việc chọn số lớn hơn\(v\), phù hợp với trực giác rằng những chiếc xe nhanh hơn luôn giành chiến thắng từ những điểm xuất phát giống hệt nhau.
