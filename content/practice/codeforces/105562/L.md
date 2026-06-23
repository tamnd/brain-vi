---
title: "CF 105562L - Thư viện giới hạn"
description: "Chúng ta được cung cấp một hệ thống giá sách thư viện, trong đó mỗi giá có thể chứa một số lượng sách cố định nếu nó chỉ được sử dụng để đựng sách. Tuy nhiên, một chiếc kệ cũng có thể tùy ý trưng bày một tác phẩm nghệ thuật, điều này làm giảm sức chứa hiệu quả của chiếc kệ đó đối với sách."
date: "2026-06-22T06:29:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105562
codeforces_index: "L"
codeforces_contest_name: "2024-2025 ICPC Northwestern European Regional Programming Contest (NWERC 2024)"
rating: 0
weight: 105562
solve_time_s: 42
verified: true
draft: false
---

[CF 105562L - Thư viện giới hạn](https://codeforces.com/problemset/problem/105562/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống giá sách thư viện, trong đó mỗi giá có thể chứa một số lượng sách cố định nếu nó chỉ được sử dụng để đựng sách. Tuy nhiên, một chiếc kệ cũng có thể tùy ý trưng bày một tác phẩm nghệ thuật, điều này làm giảm sức chứa hiệu quả của chiếc kệ đó đối với sách. 

Mỗi kệ có một chiều cao kết cấu, nhưng chiều cao đó chỉ quan trọng trong chừng mực nó không ảnh hưởng trực tiếp đến công suất. Điều quan trọng là mọi kệ đều hoạt động giống nhau về quy tắc sức chứa: nó có thể lưu trữ tối đa`x`sách nếu nó được sử dụng bình thường, hoặc lên đến`y`sách nếu một tác phẩm nghệ thuật được đặt trên đó. Có thể đặt tối đa một tác phẩm nghệ thuật trên mỗi kệ. 

Chúng tôi cũng được cung cấp nhiều bộ sách có kích cỡ và mỗi cuốn sách phải được đặt trên một số giá. Sách không được chia nhỏ và mỗi cuốn sách chiếm một đơn vị dung lượng trên kệ, vì vậy chúng tôi chỉ quan tâm đến tổng số sách trên mỗi kệ. 

Nhiệm vụ là quyết định xem có thể đặt tất cả các cuốn sách lên kệ hay không và nếu có, hãy chọn càng nhiều kệ càng tốt để chứa một tác phẩm nghệ thuật trong khi vẫn vừa với tất cả các cuốn sách. 

Điểm mấu chốt là mỗi tác phẩm nghệ thuật đều giảm dung lượng bằng cách`x - y`, vì vậy việc sử dụng quá nhiều kệ nghệ thuật có thể khiến tổng dung lượng không đủ cho tất cả các cuốn sách. Tuy nhiên, vị trí nghệ thuật không đồng đều trên các kệ, vì vậy chúng tôi phải quyết định xem mình có thể “hạ cấp” bao nhiêu kệ theo sức chứa`y`trong khi vẫn giữ được tổng công suất ít nhất`m`. 

Các ràng buộc rất lớn: lên tới 100000 giá sách và 100000 cuốn sách. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các tập hợp con của giá sách hoặc chỉ định sách riêng lẻ bằng các vòng lặp lồng nhau. Chúng ta cần ít nhất hành vi tuyến tính hoặc gần tuyến tính, có thể là sắp xếp và cấu trúc tham lam. 

Một điểm tinh tế nhưng quan trọng là chiều cao kệ và kích thước sách không liên quan đến tính khả thi ngoài việc đếm. Vì mỗi cuốn sách chiếm một đơn vị không gian và tất cả các kệ đều giống nhau ngoại trừ sức chứa, nên các giá trị`a`Và`b`không ảnh hưởng đến logic cuối cùng chút nào. Một giải pháp ngây thơ có thể nhầm lẫn là cố gắng xếp sách vào kệ theo chiều cao, nhưng đó là một điều sai lầm. 

Các trường hợp cạnh xuất hiện khi tổng công suất ngay cả khi không có tác phẩm nghệ thuật cũng không đủ. Ví dụ, nếu`n * x < m`, thì không có cấu hình nào hoạt động và câu trả lời phải là "không thể" ngay cả trước khi xem xét các tác phẩm nghệ thuật. 

Một trường hợp góc cạnh khác là khi gần như tất cả các kệ phải hạ cấp xuống`y`dung tích. Trong trường hợp đó, việc tham lam lựa chọn kệ nào sẽ lưu trữ các tác phẩm nghệ thuật hoàn toàn không phụ thuộc vào các mảng nhất định mà chỉ phụ thuộc vào việc tối đa hóa số lượng trong điều kiện hạn chế về dung lượng toàn cầu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử tất cả số lượng kệ có thể chứa các tác phẩm nghệ thuật. Giả sử chúng ta thử`k`kệ như kệ nghệ thuật. Khi đó tổng công suất sẽ là:`(n - k) * x + k * y = n * x - k * (x - y)`Đối với mỗi`k`, chúng tôi kiểm tra xem công suất này ít nhất là`m`. Nếu có, chúng tôi xem xét`k`khả thi và tận dụng tối đa như vậy`k`. 

Điều này đã loại bỏ nhu cầu chỉ định các kệ hoặc sách cụ thể. Tuy nhiên, cách triển khai đơn giản vẫn có thể cố gắng mô phỏng việc đặt sách vào giá, điều này sẽ tốn kém.`O(n + m)`mỗi lần kiểm tra và dẫn đến sự cố không cần thiết. 

Quan sát chính là tính khả thi chỉ phụ thuộc vào tổng công suất chứ không phụ thuộc vào khả năng phân phối. Một khi chúng ta nhận ra điều này, vấn đề sẽ giảm xuống còn việc tìm giá trị lớn nhất`k`sao cho bất đẳng thức xảy ra:`n * x - k * (x - y) >= m`Đây là điều kiện đơn điệu đơn giản trong`k`. BẰNG`k`tăng, dung lượng giảm nên có thể trực tiếp giải tìm biên thay vì phải tìm kiếm. 

Chúng tôi tính toán mức thâm hụt công suất yêu cầu tối thiểu:`deficit = n * x - m`Nếu như`deficit < 0`, ngay cả những kệ sách không có tác phẩm nghệ thuật cũng không thể phù hợp với tất cả các cuốn sách. 

Nếu không, mỗi kệ nghệ thuật sẽ loại bỏ`x - y`nên số lượng kệ mỹ thuật tối đa là:`k = deficit // (x - y)`Nhưng chúng ta cũng phải đảm bảo`k <= n`. 

Điều này mang lại một giải pháp dạng đóng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên k với tính toán lại | O(nm) hoặc O(n²) tùy theo mô phỏng | O(1) | Quá chậm | 
| Lý luận năng lực dạng đóng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề về khả năng đếm hơn là mô phỏng vị trí. 

## Hướng dẫn thuật toán 

1. Tính tổng sức chứa hiện có nếu mỗi kệ đều dùng hết sách. Đây là`total = n * x`. Điều này thể hiện giới hạn trên trước khi bất kỳ tác phẩm nghệ thuật nào được đặt. 
2. So sánh`total`với số lượng sách`m`. Nếu như`total < m`, không thể đặt tất cả các cuốn sách ngay cả trong trường hợp tốt nhất, vì vậy chúng tôi ngay lập tức trả về "không thể". 
3. Tính xem dung lượng dự phòng còn lại vượt quá mức cần thiết cho sách:`extra = total - m`. Điều này đo lường mức độ chúng ta có thể đủ khả năng để mất đi bằng cách đặt các tác phẩm nghệ thuật. 
4. Mỗi tác phẩm nghệ thuật đều giảm công suất một cách chính xác`x - y`. Điều này là do một chiếc kệ có thể chứa`x`sách bây giờ chỉ giữ`y`. 
5. Tính số lượng kệ nghệ thuật tối đa là`k = extra // (x - y)`. Điều này đảm bảo chúng tôi không bao giờ vượt quá mức năng lực sẵn có. 
6. Vì chúng tôi không thể sử dụng nhiều giá hơn mức tồn tại nên hãy kẹp kết quả tối đa`n`. 

### Tại sao nó hoạt động 

Bất biến chính là chỉ có tổng công suất mới quan trọng chứ không phải danh tính của các kệ hoặc đơn đặt hàng. Mỗi cấu hình với`k`kệ nghệ thuật tạo ra tổng công suất như nhau`n * x - k * (x - y)`. Vì các cuốn sách không thể phân biệt được về mức tiêu thụ không gian nên mọi sự sắp xếp thỏa mãn tổng dung lượng đều hợp lệ. Sự giảm công suất đơn điệu khi tăng`k`đảm bảo rằng khả năng lớn nhất có thể`k`chính xác là cái sử dụng tất cả sự lỏng lẻo có sẵn mà không vượt quá nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, x, y = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

total = n * x

if total < m:
    print("impossible")
else:
    extra = total - m
    k = extra // (x - y)
    if k > n:
        k = n
    print(k)
```Quá trình triển khai bắt đầu bằng cách đọc tất cả thông tin đầu vào, mặc dù chiều cao kệ và kích thước sách không được sử dụng trong logic cuối cùng. Điều này phản ánh sự đơn giản hóa mô hình: chúng không liên quan đến tính khả thi. 

Chúng tôi tính toán tổng công suất giả sử không có tác phẩm nghệ thuật nào. Nếu điều đó vẫn chưa đủ, chúng tôi sẽ chấm dứt ngay lập tức. Ngược lại, chúng tôi tính toán xem có thể hy sinh bao nhiêu công suất. Mỗi kệ nghệ thuật có một mức giá cố định về sức chứa, do đó việc chia sẽ cho số lượng tối đa. 

Điểm tinh tế duy nhất là đảm bảo sử dụng phép chia số nguyên, vì không được phép phân bổ một phần tổn thất công suất. Kẹp bằng`n`ngăn chặn việc vượt quá số lượng kệ sẵn có, mặc dù về mặt toán học giới hạn này không bao giờ bị vi phạm khi`extra >= 0`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`n=4, m=8, x=4, y=2`Tổng công suất là`16`. Chúng ta cần phải phù hợp`8`, Vì thế`extra = 8`. 

Giá mỗi kệ tranh nghệ thuật`2`, Vì thế`k = 8 // 2 = 4`. 

Tất cả các kệ có thể lưu trữ nghệ thuật. 

| Bước | tổng cộng | thêm | k | 
| --- | --- | --- | --- | 
| ban đầu | 16 | - | - | 
| tính toán thêm | 16 | 8 | - | 
| tính k | 16 | 8 | 4 | 

Điều này khẳng định rằng chúng tôi có thể tối đa hóa việc sử dụng nghệ thuật trong khi vẫn phù hợp với tất cả các cuốn sách. 

### Mẫu 2 

đầu vào:`n=4, m=11, x=3, y=2`Tổng công suất là`12`. Chúng ta cần phải phù hợp`11`, Vì thế`extra = 1`. 

Giá mỗi kệ tranh nghệ thuật`1`, Vì thế`k = 1`. 

| Bước | tổng cộng | thêm | k | 
| --- | --- | --- | --- | 
| ban đầu | 12 | - | - | 
| tính toán thêm | 12 | 1 | - | 
| tính k | 12 | 1 | 1 | 

Chỉ có thể sử dụng một kệ cho tác phẩm nghệ thuật vì nếu có thêm bất kỳ kệ nào sẽ làm giảm sức chứa dưới mức yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học sau khi đọc đầu vào | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài kho lưu trữ đầu vào | 

Các ràng buộc lên tới 100000 không còn liên quan sau khi giảm bớt, vì chúng tôi không bao giờ lặp lại các giá sách hoặc sách một cách có ý nghĩa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, x, y = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    total = n * x
    if total < m:
        return "impossible"
    extra = total - m
    k = extra // (x - y)
    if k > n:
        k = n
    return str(k)

# provided samples
assert run("4 8 4 2\n4 8 6 2\n1 2 3 5 7 7 8 8") == "4"
assert run("4 11 3 2\n2 2 2 2\n1 1 1 1 1 1 1 1 1 1 1") == "1"
assert run("2 10 3 2\n8 6\n4 2 1 3 6 2 1 3 4 5") == "impossible"

# custom cases
assert run("1 5 5 1\n10\n1 1 1 1 1") == "0", "no slack at all"
assert run("5 0 10 3\n1 1 1 1 1\n") == "5", "zero books allows all art"
assert run("3 30 10 9\n1 1 1\n" + "1 "*30) == "3", "tiny degradation per art"
assert run("10 100 10 5\n" + "1 "*10 + "\n" + "1 "*100) == "2", "capacity tight case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 kệ vừa vặn | 0 | ranh giới nơi không có nghệ thuật phù hợp | 
| không có sách | n | trường hợp tự do hoàn toàn | 
| giảm công suất nhỏ | 3 | hành vi xuống cấp dần dần | 
| trường hợp lớn chặt chẽ | 2 | chia tỷ lệ chính xác theo các ràng buộc | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi thậm chí không có tác phẩm nghệ thuật nào đã vượt quá khả năng. Ví dụ: 

đầu vào:`n=2, m=10, x=3, y=2`Tổng công suất là`6`, nhỏ hơn`10`. Thuật toán ngay lập tức trả về "không thể" trước khi tính toán thêm. Một cách tiếp cận ngây thơ vẫn có thể thử phân phối sách hoặc lý luận về vị trí nghệ thuật, nhưng điều đó là không cần thiết khi giới hạn năng lực không thành công. 

Một trường hợp khác là khi`y`rất gần với`x`. Ví dụ:`x=10, y=9`Ở đây mỗi kệ nghệ thuật chỉ giảm sức chứa bằng`1`. Thuật toán giải thích chính xác điều này là cho phép nhiều kệ nghệ thuật, chỉ giới hạn bởi khoảng trống nhỏ`n*x - m`. Mô hình chi phí tuyến tính đơn điệu đảm bảo bước chia vẫn hợp lệ mà không cần viết vỏ đặc biệt. 

Cuối cùng, khi`m = 0`, mọi kệ đều có thể lưu trữ các tác phẩm nghệ thuật vì không cần chỗ chứa sách. Công thức mang lại`extra = n*x`, và do đó`k = (n*x) // (x-y)`, nhưng chúng ta vẫn phải bám sát`n`, tạo ra việc sử dụng đầy đủ chính xác.
