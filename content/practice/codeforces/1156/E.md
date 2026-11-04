---
title: "CF 1156E - Phân đoạn hoán vị đặc biệt"
description: "Chúng ta được hoán vị các số từ 1 đến n. Với mỗi mảng con [l, r], chúng ta xét giá trị lớn nhất của nó. Mảng con được gọi là đặc biệt khi tổng của hai giá trị điểm cuối bằng giá trị tối đa đó: $$pl + pr = max(pl,dots,pr)$$ Nhiệm vụ là đếm xem có bao nhiêu mảng con…"
date: "2026-06-12T02:39:17+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dsu", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1156
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 64 (Rated for Div. 2)"
rating: 2200
weight: 1156
solve_time_s: 101
verified: true
draft: false
---

[CF 1156E - Phân đoạn hoán vị đặc biệt](https://codeforces.com/problemset/problem/1156/E) 

**Đánh giá:** 2200 
**Thẻ:** cấu trúc dữ liệu, chia để trị, dsu, hai con trỏ 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số từ`1`ĐẾN`n`. Đối với mỗi mảng con`[l, r]`, chúng ta xét giá trị lớn nhất của nó. Mảng con được gọi là đặc biệt khi tổng của hai giá trị điểm cuối bằng giá trị tối đa đó:$$p_l + p_r = \max(p_l,\dots,p_r)$$Nhiệm vụ là đếm xem có bao nhiêu mảng con thỏa mãn điều kiện này. 

Mảng là một hoán vị, là thuộc tính quan trọng. Mỗi giá trị xuất hiện chính xác một lần. Phần tử lớn nhất của bất kỳ đoạn nào được xác định duy nhất và xuất hiện ở đúng một vị trí. 

Ràng buộc`n ≤ 2 · 10^5`ngay lập tức loại trừ bất kỳ thuật toán nào kiểm tra tất cả các mảng con. có khoảng$$\frac{n(n+1)}2 \approx 2 \cdot 10^{10}$$mảng con trong trường hợp xấu nhất, vượt xa những gì có thể được xử lý trong hai giây. Thậm chí một`O(n²)`giải pháp sẽ thực hiện xung quanh`4 · 10^10`hoạt động. 

Thuộc tính hoán vị gợi ý rằng các giá trị có thể được sử dụng làm mã định danh và việc tra cứu vị trí có thể được thực hiện một cách hiệu quả. Bất kỳ giải pháp được chấp nhận nào cũng sẽ cần một cái gì đó gần với`O(n log n)`. 

Một điểm tinh tế là phần tử tối đa có thể nằm ở bất kỳ đâu trong đoạn, không nhất thiết phải ở một trong các điểm cuối. 

Coi như:```
3 4 1
```Phân khúc`[1,3]`là đặc biệt bởi vì$$3 + 1 = 4$$Và`4`là mức tối đa của phân khúc, mặc dù nó ở giữa. 

Một sai lầm dễ mắc phải khác là đếm các cặp giá trị thay vì các cặp vị trí. 

Ví dụ:```
2 1 4 3
```Các giá trị`1`Và`3`tổng hợp thành`4`, nhưng đoạn chứa vị trí của chúng là`[2,4]`, giá trị cực đại của nó vẫn là`4`, vì vậy cặp này đóng góp. Điều kiện phụ thuộc vào cả giá trị và vị trí kèm theo chúng. 

Cạm bẫy thứ ba là tính hai lần. Giả sử mức tối đa của một phân khúc là`M`. Vì mảng là một hoán vị nên chỉ có một vị trí chứa`M`và mọi phân đoạn hợp lệ đều được liên kết tự nhiên với vị trí tối đa duy nhất đó. Một giải pháp đúng phải đếm từng đoạn chính xác một lần cho đến mức tối đa của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê mọi mảng con. Đối với mỗi cặp`(l,r)`chúng tôi tính toán giá trị tối đa trên phân đoạn đó và kiểm tra xem tổng điểm cuối có bằng mức tối đa đó hay không. 

Bản thân điều kiện này rất dễ xác minh, nhưng có`Θ(n²)`phân đoạn. Ngay cả khi các truy vấn phạm vi tối đa đã được trả lời trong`O(1)`, chúng tôi vẫn sẽ thực hiện khoảng 20 tỷ lần kiểm tra khi`n = 200000`. Điều này là chậm một cách vô vọng. 

Quan sát quan trọng là mỗi phân đoạn đặc biệt đều có một phần tử tối đa duy nhất. Đặt giá trị tối đa của một phân đoạn đặc biệt là`x`, nằm ở vị trí`m`. 

Bởi vì$$p_l + p_r = x,$$điểm cuối phải là hai giá trị có tổng chính xác`x`. 

Bây giờ hãy nhìn vào tất cả các phân đoạn có giá trị tối đa là`x`. Từ`x`là lớn nhất thì mọi phần tử bên trong đoạn đó phải nhỏ hơn`x`. Điều này có nghĩa là đoạn phải nằm trong khoảng tối đa xung quanh vị trí`m`chỉ chứa các giá trị nhỏ hơn`x`. 

Điều này đương nhiên dẫn đến sự phân chia và chinh phục vị trí của phần tử tối đa. 

Giả sử chúng ta xử lý đệ quy một khoảng`[L,R]`. Cho phép`m`là vị trí có giá trị lớn nhất trong khoảng đó. 

Bất kỳ phân đoạn đặc biệt nào có mức tối đa là`p[m]`phải có một điểm cuối ở bên trái của`m`và cái còn lại ở bên phải`m`(có thể một điểm cuối bằng`m`). Với mọi giá trị`a`một mặt, chúng ta cần một giá trị$$b = p[m] - a$$ở phía bên kia. 

Vì mảng là một hoán vị nên mọi giá trị đều xuất hiện ở đúng một vị trí. Nếu chúng ta biết vị trí của mọi giá trị, chúng ta có thể kiểm tra`O(1)`liệu giá trị bổ sung có tồn tại ở phía đối diện hay không. 

Để giữ độ phức tạp ở mức thấp, chúng tôi luôn lặp qua cạnh nhỏ hơn của giá trị tối đa và tìm kiếm phần bù ở phía lớn hơn. Đây là thủ thuật đếm chia để trị cổ điển mang lại kết quả$$O(n \log n)$$tổng thể. 

Cấu trúc đệ quy giống hệt cây Descartes của hoán vị. Mỗi mức phí hoạt động tỷ lệ thuận với phía nhỏ hơn và mọi phần tử chỉ tham gia vào`O(log n)`những lần quét như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Phân chia và chinh phục tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tiền xử lý 

cửa hàng`pos[v]`, vị trí của giá trị`v`trong hoán vị. 

Đồng thời xây dựng cấu trúc có thể trả về vị trí của giá trị lớn nhất trong bất kỳ khoảng nào. Cây phân đoạn hoạt động tốt. 

### Giải đệ quy(L, R) 

1. Nếu`L >= R`, trở lại. 
2. Tìm vị trí`m`có giá trị lớn nhất trong`[L,R]`. 
3. Hãy để`mx = p[m]`. 
4. So sánh kích thước bên trái`[L,m-1]`và bên phải`[m+1,R]`. 
5. Lặp lại phần nhỏ hơn. 
6. Với mỗi giá trị`a`gặp phải, tính toán$$b = mx - a.$$1. Nếu`b`nằm ngoài phạm vi`1..n`, bỏ qua nó. 
2. Tra cứu vị trí của`b`sử dụng`pos[b]`. 
3. Kiểm tra xem vị trí đó có nằm ở phía đối diện của`m`nhưng vẫn ở bên trong`[L,R]`. 
4. Nếu đúng như vậy thì chúng ta đã tìm thấy chính xác một đoạn đặc biệt có giá trị lớn nhất là`mx`, vì vậy hãy thêm một vào câu trả lời. 
5. Xử lý đệ quy`[L,m-1]`. 
6. Xử lý đệ quy`[m+1,R]`. 

### Tại sao nó hoạt động 

Sửa bất kỳ phân khúc đặc biệt nào. Gọi giá trị lớn nhất của nó là`mx`ở vị trí`m`. 

Khi đệ quy đạt tới khoảng nhỏ nhất chứa đoạn đó và có giá trị lớn nhất là`mx`, điểm cuối của đoạn nằm ở phía đối diện của`m`hoặc một điểm cuối bằng`m`. Giá trị của chúng thỏa mãn$$a+b=mx.$$Trong quá trình quét mặt nhỏ hơn, một giá trị điểm cuối sẽ được kiểm tra. Thuật toán tính giá trị bổ sung`mx-a`và kiểm tra xem vị trí của nó có nằm ở phía bên kia hay không. Đoạn này được tính một lần. 

Không có phân đoạn nào có thể được tính hai lần vì mỗi phân đoạn có một giá trị tối đa duy nhất và do đó thuộc về chính xác một nút đệ quy. Ngược lại, mỗi cặp được đếm tương ứng với các điểm cuối có phân đoạn kèm theo có giá trị tối đa`mx`, vì vậy mọi số đếm đều hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def sol
```
