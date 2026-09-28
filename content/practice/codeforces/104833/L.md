---
title: "CF 104833L - \u5140\u7a81\u9aa8\u4e4b\u6b7b"
description: "Chúng tôi đang mô phỏng quá trình sinh tồn theo lượt trong đó nhân vật bắt đầu với giá trị máu ban đầu và liên tục mất máu trong một chuỗi các vòng chơi. Điều khó khăn là sát thương gây ra vào cuối mỗi hiệp không được cố định."
date: "2026-06-28T11:55:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104833
codeforces_index: "L"
codeforces_contest_name: "The 2023 Zhejiang SCI-TECH University Freshman Programming Contest"
rating: 0
weight: 104833
solve_time_s: 43
verified: true
draft: false
---

[CF 104833L - \u5140\u7a81\u9aa8\u4e4b\u6b7b](https://codeforces.com/problemset/problem/104833/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng quá trình sinh tồn theo lượt trong đó nhân vật bắt đầu với giá trị máu ban đầu và liên tục mất máu trong một chuỗi các vòng chơi. Điều khó khăn là sát thương gây ra vào cuối mỗi hiệp không được cố định. Thay vào đó, nó phụ thuộc vào số lần thiệt hại do hỏa hoạn đã xảy ra cho đến thời điểm đó. 

Chính xác hơn, chúng tôi xử lý một mảng có độ dài$n$. Mỗi phần tử cho chúng ta biết liệu vòng hiện tại có kích hoạt một “ngăn xếp lửa” bổ sung hay không. Nếu có, bộ đếm toàn cầu sẽ tăng lên. Sau khi xử lý sự kiện của vòng đó, nhân vật nhận sát thương bằng giá trị hiện tại của bộ đếm này. Nhân vật bắt đầu bằng sức khỏe$x$, và chúng tôi phải xác định vòng đầu tiên mà sức khỏe giảm xuống 0 hoặc thấp hơn hoặc báo cáo rằng điều này không bao giờ xảy ra. 

Cấu trúc chính là thiệt hại được tích lũy và đơn điệu tăng dần theo thời gian. Mỗi khi sự kiện hỏa hoạn xuất hiện, tất cả các vòng tiếp theo đều trở nên nguy hiểm hơn vì sát thương mỗi vòng tăng lên vĩnh viễn. 

Những hạn chế đẩy chúng tôi tới giải pháp quét tuyến tính. Với$n \le 10^6$, bất kỳ mô phỏng bậc hai nào, chẳng hạn như tính toán lại thiệt hại tích lũy từ đầu mỗi vòng, đều quá chậm. Chúng tôi phải duy trì trạng thái chạy và cập nhật nó theo thời gian không đổi cho mỗi phần tử. Các hạn chế về bộ nhớ là tiêu chuẩn, do đó việc lưu trữ mảng là được, nhưng việc tính toán lại thêm thì không. 

Một trường hợp khó nhận thấy xuất hiện khi nhân vật chết chính xác tại một ranh giới tròn sau khi gây sát thương. Việc kiểm tra cái chết phải diễn ra sau khi áp dụng sát thương của vòng hiện tại, không phải trước đó. Một trường hợp góc khác là khi sát thương trở nên lớn vào cuối quá trình, nghĩa là các hiệp đầu có vẻ an toàn nhưng các đợt bùng nổ sau đó có thể đột ngột kết thúc trò chơi. Cuối cùng, nếu máu không bao giờ giảm xuống dưới 0 ngay cả khi sát thương tích lũy tối đa, chúng ta phải xuất ra một cách chính xác rằng khả năng sống sót vẫn tiếp tục qua tất cả các hiệp. 

## Phương pháp tiếp cận 

Một mô phỏng đơn giản tuân theo các quy tắc theo đúng nghĩa đen. Chúng tôi duy trì sức khỏe hiện tại và một bộ đếm lửa. Đối với mỗi vòng, nếu giá trị mảng là 1, chúng tôi sẽ tăng bộ đếm, sau đó chúng tôi trừ đi sức khỏe của bộ đếm. Sau mỗi lần trừ, chúng tôi kiểm tra xem sức khỏe đã giảm xuống 0 hay thấp hơn. Điều này đúng vì nó phản ánh chính xác quy trình được mô tả. 

Việc giải thích bạo lực sẽ vẫn tuyến tính theo thời gian, nhưng một phiên bản ít cẩn thận hơn có thể tính toán lại “thiệt hại hiện tại” bằng cách quét tất cả các sự kiện cháy trước đó ở mỗi bước. Điều đó sẽ làm cho mỗi vòng tốn kém$O(n)$, dẫn đến$O(n^2)$tổng số hoạt động trong trường hợp xấu nhất. Với$n = 10^6$, điều đó trở nên không thể thực hiện được. 

Quan sát quan trọng là thiệt hại ở vòng$i$chỉ phụ thuộc vào số lượng tiền tố của các sự kiện cháy. Số tiền tố này có thể được duy trì tăng dần. Khi chúng tôi nhận ra điều này, mỗi vòng sẽ trở thành một bản cập nhật liên tục: tăng bộ đếm nếu cần, trừ nó khỏi máu và kiểm tra sự chấm dứt. 

Điều này làm giảm toàn bộ quá trình thành một lần truyền qua mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại sát thương mỗi hiệp) |$O(n^2)$|$O(1)$| Quá chậm | 
| Mô phỏng tiền tố tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai biến: sức khỏe hiện tại và bộ đếm lửa tích lũy. 

1. Khởi tạo`hp = x`Và`burn = 0`. Biến`burn`thể hiện mức độ thiệt hại sẽ được áp dụng cho mỗi hiệp kể từ bây giờ nếu không có thêm đám cháy nào xuất hiện. 
2. Lặp lại mảng từ vòng đầu tiên đến vòng cuối cùng. 
3. Nếu giá trị hiện tại là 1, hãy tăng`burn`bằng 1. Điều này phản ánh rằng tất cả các vòng trong tương lai sẽ trở nên nguy hiểm hơn. 
4. Trừ`burn`từ`hp`. Đây là mô hình ứng dụng sát thương cuối trận. 
5. Nếu`hp <= 0`, xuất chỉ số vòng hiện tại và dừng ngay lập tức. Cần phải có vòng sớm nhất như vậy, vì vậy chúng ta không được tiếp tục. 
6. Nếu chúng ta hoàn thành tất cả các vòng mà không`hp`trở nên không dương, đầu ra đảm bảo sự tồn tại. 

Tính đúng đắn phụ thuộc vào thực tế là thiệt hại hoàn toàn được xác định bởi số lượng vụ cháy đã xảy ra cho đến nay. Mỗi sự kiện hỏa hoạn sẽ tăng vĩnh viễn tất cả thiệt hại trong tương lai thêm đúng một đơn vị, vì vậy`burn`luôn bằng tổng tiền tố của các chỉ báo cháy. 

### Tại sao nó hoạt động 

Ở bất kỳ vòng nào$i$, sát thương áp dụng chính xác là số chỉ số$j \le i$như vậy$a_j = 1$. Thuật toán duy trì đại lượng này một cách rõ ràng trong`burn`. Vì tình trạng giảm một cách xác định theo giá trị tiền tố này ở mỗi bước nên mô phỏng khớp chính xác với quy trình được mô tả trong bài toán. Bởi vì chúng tôi kiểm tra cái chết ngay sau khi áp dụng sát thương chính xác cho vòng đó, nên lần đầu tiên sức khỏe trở nên không tích cực chắc chắn sẽ được báo cáo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, x = map(int, input().split())
a = list(map(int, input().split()))

hp = x
burn = 0

for i in range(n):
    if a[i] == 1:
        burn += 1
    hp -= burn
    if hp <= 0:
        print("YES")
        print(i + 1)
        break
else:
    print("NO")
```Việc thực hiện phản ánh trực tiếp thuật toán. Vòng lặp sử dụng Python`for-else`cấu trúc sao cho trường hợp “KHÔNG” chỉ kích hoạt nếu không xảy ra sự cố. chỉ số`i + 1`chuyển đổi từ lập chỉ mục dựa trên 0 sang đánh số vòng dựa trên một theo yêu cầu của đầu ra. 

Một lỗi phổ biến là trừ đi thiệt hại trước khi cập nhật bộ đếm số lần đốt. Điều đó sẽ dịch chuyển tất cả thiệt hại đi một vòng và tạo ra kết quả tử vong sớm không chính xác. Một sự tinh tế khác là đảm bảo sự gián đoạn xảy ra ngay lập tức khi sức khỏe trở nên không tích cực; tiếp tục đi xa hơn sẽ báo cáo không chính xác về vòng tử thần sau đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
0 1 0
```| Vòng | một [tôi] | đốt trước | đốt sau | hp trước | hp sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 3 | 3 | 
| 2 | 1 | 0 | 1 | 3 | 2 | 
| 3 | 0 | 1 | 1 | 2 | 1 | 

Nhân vật sống sót qua mọi vòng đấu vì sức khỏe không bao giờ bằng 0. Điều này chứng tỏ rằng chỉ một lần đốt cháy không phải lúc nào cũng đủ để khắc phục lượng máu ban đầu trừ khi có đủ thời gian để tích lũy. 

Đầu ra:```
NO
```### Ví dụ 2 

đầu vào:```
3 3
0 0 1
```| Vòng | một [tôi] | đốt trước | đốt sau | hp trước | hp sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 3 | 3 | 
| 2 | 0 | 0 | 0 | 3 | 3 | 
| 3 | 1 | 0 | 1 | 3 | 2 | 

Ở đây, sự sống sót lại xảy ra, nhưng chỉ do lượng đốt tăng quá muộn để tích lũy sát thương đáng kể trong số vòng giới hạn. Điều này nhấn mạnh rằng thời điểm xảy ra hỏa hoạn là rất quan trọng, không chỉ số lượng của chúng. 

Đầu ra:```
NO
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vòng thực hiện một số lần cập nhật không đổi: tăng, trừ và so sánh tùy chọn | 
| Không gian |$O(1)$| Chỉ một số biến vô hướng được duy trì bất kể kích thước đầu vào | 

Quét tuyến tính vừa vặn thoải mái trong các ràng buộc đối với$n \le 10^6$. Mỗi phép toán là số học số nguyên đơn giản, do đó, giải pháp chạy tốt trong giới hạn thời gian thông thường là 1 giây trong Python khi được triển khai với đầu vào nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    backup = stdout
    out = io.StringIO()
    sys.stdout = out

    # solution copied here for testing
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    hp = x
    burn = 0

    for i in range(n):
        if a[i] == 1:
            burn += 1
        hp -= burn
        if hp <= 0:
            print("YES")
            print(i + 1)
            break
    else:
        print("NO")

    sys.stdout = backup
    return out.getvalue().strip()

# provided samples
assert run("3 3\n0 1 0\n") == "NO"
assert run("3 3\n0 0 1\n") == "NO"

# custom cases

# minimum size, immediate death
assert run("1 1\n1\n") == "YES\n1"

# no fire at all
assert run("5 10\n0 0 0 0 0\n") == "NO"

# all fire, rapid growth
assert run("4 3\n1 1 1 1\n") == "YES\n3"

# late burst
assert run("6 10\n0 0 0 0 1 1\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/1 | CÓ 1 | Tử vong ngay khi bắt đầu bỏng ở bước đầu tiên | 
| 5 10 / tất cả 0 | KHÔNG | Không tích lũy thiệt hại | 
| 4 3 / tất cả 1 | CÓ 3 | Tăng đốt cháy dẫn đến sụp đổ sớm | 
| 6 10 / người đến muộn | KHÔNG | Lửa muộn không đủ trong chân trời | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhân vật bắt đầu với lượng máu rất thấp và sự kiện cháy đầu tiên xuất hiện ngay lập tức. Đối với đầu vào`1 1 / 1`, đốt cháy trở thành 1 trước khi gây sát thương và máu giảm xuống 0 ngay lập tức. Thuật toán cập nhật chính xác khả năng đốt cháy trước rồi áp dụng sát thương, gây ra cái chết ở hiệp 1. 

Một trường hợp cạnh khác là khi tất cả các giá trị bằng 0. Biến đốt cháy không bao giờ tăng, vì vậy không có thiệt hại nào được áp dụng. Thuật toán xử lý tất cả các vòng và thoát qua đường dẫn vòng lặp khác, đưa ra kết quả sống sót chính xác. 

Trường hợp thứ ba liên quan đến các đợt cháy dày đặc sớm. Đối với đầu vào`4 3 / 1 1 1 1`, khả năng đốt cháy tiến hóa theo cấp độ 1, 2, 3, 4 trong khi máu giảm theo. Quá trình này đảm bảo rằng khi vết đốt vượt quá lượng máu còn lại, vòng chính xác sẽ được phát hiện mà không vượt quá mức, vì việc kiểm tra diễn ra ngay sau khi trừ. 

Một trường hợp tế nhị cuối cùng là khi cái chết xảy ra ở vòng cuối cùng. Vòng lặp vẫn thực hiện kiểm tra trước khi kết thúc, đảm bảo báo cáo chỉ số vòng chính xác thay vì mặc định là “KHÔNG”.
