---
title: "CF 1075A - Cuộc đua của nhà vua"
description: "Chúng ta được cung cấp một lưới $n lần n$. Một quân vua bắt đầu ở góc dưới bên trái $(1,1)$ và quân vua kia bắt đầu ở góc trên bên phải $(n,n)$. Một đồng xu được đặt ở mức $(x,y)$ và cả hai vị vua đều cố gắng tiếp cận nó càng nhanh càng tốt."
date: "2026-06-15T06:54:50+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1075
codeforces_index: "A"
codeforces_contest_name: "Lyft Level 5 Challenge 2018 - Final Round (Open Div. 2)"
rating: 800
weight: 1075
solve_time_s: 144
verified: true
draft: false
---

[CF 1075A - Cuộc đua của nhà vua](https://codeforces.com/problemset/problem/1075/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới. Một vị vua bắt đầu ở góc dưới bên trái$(1,1)$và cái còn lại bắt đầu ở góc trên bên phải$(n,n)$. Một đồng xu được đặt ở$(x,y)$, và cả hai vị vua đều cố gắng đạt được nó càng nhanh càng tốt. Họ di chuyển giống như một vị vua cờ vua, vì vậy trong một nước đi, họ có thể đi đến bất kỳ ô nào trong số tám ô lân cận, kể cả đường chéo. 

Đại lượng quan trọng trong bài toán này là số bước đi tối thiểu mà quân vua cần để đến được ô mục tiêu. Vì được phép di chuyển theo đường chéo nên quân vua giảm thiểu sự khác biệt đồng thời của cả hàng và cột một cách hiệu quả khi có thể. Điều này làm cho khoảng cách giữa hai ô bằng khoảng cách Chebyshev:$$\text{dist}((a,b),(c,d)) = \max(|a-c|, |b-d|)$$Vua trắng đi trước, sau đó đến quân đen, luân phiên lượt. Mỗi nước đi mất đúng một bước khoảng cách đối với vua di chuyển. Người chiến thắng là người đạt được$(x,y)$Đầu tiên. 

Những ràng buộc cho phép$n$lên tới$10^{18}$, vì vậy bất kỳ mô phỏng chuyển động từng bước nào đều không thể thực hiện được. Chúng ta phải tính khoảng cách trong thời gian không đổi. 

Một trường hợp phức tạp là khi quân vua bắt đầu trên đồng xu. Nếu như$x,y = 1,1$, quân trắng thắng ngay trước khi quân đen đi. Nếu như$x,y = n,n$, đen thắng ngay. Bất kỳ giải pháp nào cũng phải xử lý những chiến thắng tức thời này một cách chính xác. 

Một trường hợp khác là tính chẵn lẻ: ngay cả khi quân đen ở gần hơn về khoảng cách tuyệt đối, quân trắng vẫn có thể thắng nếu quân đen cần thêm một nước đi nhưng lại chơi thứ hai. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ di chuyển cả hai vị vua từng bước một, luôn chọn bất kỳ bước đi ngắn nhất nào về phía đồng xu. Mỗi vị vua sẽ cần$O(n)$di chuyển trong trường hợp xấu nhất và mỗi di chuyển là thời gian không đổi, dẫn đến$O(n)$mỗi vị vua. Từ$n$có thể$10^{18}$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là lối chơi tối ưu mang tính quyết định: cả hai vua luôn di chuyển dọc theo con đường ngắn nhất, do đó thời gian đến của chúng hoàn toàn được xác định bởi khoảng cách ban đầu của chúng tới điểm đến.$(x,y)$. Sự tinh tế duy nhất còn lại là thứ tự lần lượt. 

Chúng tôi tính toán: 

-$d_w = \max(|x-1|, |y-1|)$-$d_b = \max(|n-x|, |n-y|)$Trắng đến đúng lúc$2 \cdot d_w$hay chính xác hơn$2 \cdot d_w$các lượt của dòng thời gian trận đấu, nhưng vì quân trắng đi trước nên lượt đến hiệu quả của quân trắng là$2d_w - 1$. 

Đen lần lượt đến$2d_b$. 

Vì vậy, so sánh giảm xuống:$$2d_w - 1 < 2d_b$$Nếu đúng thì trắng thắng, ngược lại đen thắng. 

Điều này làm giảm toàn bộ vấn đề khi tính toán hai khoảng cách Chebyshev và so sánh chúng với một dịch chuyển chẵn lẻ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| So sánh khoảng cách + chẵn lẻ |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách của vua trắng đến đồng xu bằng phép đo Chebyshev. Điều này thể hiện số lần di chuyển tối thiểu cần thiết nếu quân trắng chơi tối ưu. 
2. Tính khoảng cách của vua đen đến đồng tiền theo cách tương tự, bắt đầu từ$(n,n)$. 
3. Chuyển cả hai khoảng cách thành “lần lượt khi chạm tới đồng xu”. Trắng lần lượt đạt tới$1,3,5,\dots$, vậy lượt đến của anh ta là$2d_w - 1$. Màu đen lần lượt đạt tới$2,4,6,\dots$, vậy là của anh ấy$2d_b$. 
4. So sánh hai giá trị này. Nếu lượt đến của quân trắng nhỏ hơn thì quân trắng sẽ thắng. Ngược lại đen sẽ thắng. 

Lý do bước 3 hợp lệ là vì cả hai người chơi đều giảm khoảng cách của họ chính xác một lần cho mỗi nước đi và không nước đi nào có thể giảm nhiều hơn một đơn vị khoảng cách Chebyshev. 

### Tại sao nó hoạt động 

Điều bất biến chính là khoảng cách của mỗi vị vua tới mục tiêu giảm đi đúng một điểm cho mỗi nước đi trong lối chơi tối ưu và không có chiến lược thay thế nào có thể giảm khoảng cách đó nhanh hơn. Vì thứ tự nước đi là cố định (màu trắng luôn bắt đầu), nên sự bất đối xứng duy nhất là lợi thế một nước đi của màu trắng. Việc chuyển đổi khoảng cách thành chỉ số rẽ nắm bắt chính xác lợi thế này, vì vậy việc so sánh$2d_w - 1$Và$2d_b$hoàn toàn xác định được người chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    x, y = map(int, input().split())

    dw = max(abs(x - 1), abs(y - 1))
    db = max(abs(n - x), abs(n - y))

    white_turn = 2 * dw - 1
    black_turn = 2 * db

    if white_turn < black_turn:
        print("White")
    else:
        print("Black")

if __name__ == "__main__":
    solve()
```Khoảng cách trắng và đen được tính toán bằng hệ mét Chebyshev vì quân vua có thể di chuyển theo đường chéo, điều này làm giảm cả hai tọa độ cùng một lúc. Việc chuyển đổi sang chỉ số lần lượt là rất quan trọng: nước đi đầu tiên của quân trắng mang lại cho anh ta lợi thế một bước trong chuỗi xen kẽ, lợi thế này được ghi lại bằng cách trừ đi một bước trong thời gian đến cuối cùng của anh ta. 

Việc so sánh rất chặt chẽ vì nếu cả hai đến cùng một lượt, quân đen được coi là thắng do đi về thứ hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 3
```| Số lượng | Giá trị | 
| --- | --- | 
|$d_w$| 2 | 
|$d_b$| 2 | 
| Quay trắng | 3 | 
| Lượt đen | 4 | 

Trắng đến sớm hơn nên thắng. 

Điều này cho thấy trường hợp cả hai quân vua đều ở khoảng cách xa như nhau, nhưng thứ tự di chuyển mang lại lợi thế cho quân trắng. 

### Ví dụ 2 

đầu vào:```
5
3 5
```| Số lượng | Giá trị | 
| --- | --- | 
|$d_w$| 2 | 
|$d_b$| 2 | 
| Quay trắng | 3 | 
| Lượt đen | 4 | 

Mặc dù mục tiêu ở gần khu vực trên cùng bên phải hơn, màu trắng vẫn đến sớm hơn do thứ tự di chuyển, củng cố rằng chỉ có thời gian rẽ tương đối mới quan trọng chứ không phải độ lệch hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số phép tính số học được thực hiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng thỏa mãn các ràng buộc vì nó thực hiện tính toán theo thời gian không đổi bất kể$n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    x, y = map(int, input().split())

    dw = max(abs(x - 1), abs(y - 1))
    db = max(abs(n - x), abs(n - y))

    white_turn = 2 * dw - 1
    black
```
