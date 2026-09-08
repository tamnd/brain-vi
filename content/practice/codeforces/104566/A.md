---
title: "CF 104566A - Tình yêu sống"
description: "Chúng ta được cung cấp một chuỗi giống như nhị phân, nhưng thay vì các bit, nó bao gồm hai nhãn: HOÀN HẢO và KHÔNG HOÀN HẢO. Chuỗi có độ dài cố định n và chính xác m vị trí của nó phải HOÀN HẢO trong khi n - m còn lại là KHÔNG HOÀN HẢO."
date: "2026-06-30T08:31:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104566
codeforces_index: "A"
codeforces_contest_name: "The 2018 ACM-ICPC Asia Qingdao Regional Contest, Online (The 2nd Universal Cup. Stage 1: Qingdao)"
rating: 0
weight: 104566
solve_time_s: 50
verified: true
draft: false
---

[CF 104566A - Tình yêu sống động](https://codeforces.com/problemset/problem/104566/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi giống như nhị phân, nhưng thay vì các bit, nó bao gồm hai nhãn: HOÀN HẢO và KHÔNG HOÀN HẢO. Trình tự có độ dài cố định`n`, và chính xác`m`các vị trí của nó phải HOÀN HẢO trong khi các vị trí còn lại`n - m`KHÔNG HOÀN HẢO. 

Đối với bất kỳ sự sắp xếp hợp lệ nào của các nhãn này, chúng tôi xác định điểm của nó là độ dài của khối HOÀN THÀNH liền kề dài nhất. Nói cách khác, chúng tôi xem xét từng lần chạy của các mục HOÀN HẢO liên tiếp và lấy độ dài tối đa trong số đó. 

Nhiệm vụ không phải là xây dựng một chuỗi mà là suy luận về tất cả các chuỗi có thể thỏa mãn các ràng buộc và xác định hai thái cực: lần chạy HOÀN HẢO dài nhất có thể tối đa và lần chạy HOÀN HẢO dài nhất có thể tối thiểu. 

Các ràng buộc rất nhỏ: cả hai`n`Và`m`nhiều nhất là 1000 và có nhiều nhất 100 ca kiểm thử. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào xây dựng hoặc mô phỏng trình tự một cách rõ ràng đều không cần thiết. Cấu trúc của bài toán hoàn toàn mang tính tổ hợp và chỉ phụ thuộc vào cách chúng ta phân vùng`m`các mục giống hệt nhau (HOÀN THÀNH) thành các phân đoạn được phân tách bằng KHÔNG HOÀN THÀNH. 

Một cách tiếp cận ngây thơ có thể cố gắng liệt kê tất cả các vị trí của`m`HOÀN HẢO trong số`n`các vị trí, đó là`C(n, m)`cấu hình. Ngay cả đối với`n = 1000`, đây là một vùng thiên văn lớn và không thể khám phá được. Một ý tưởng ngây thơ khác là cố gắng đặt các hàm HOÀN THÀNH một cách tham lam và mô phỏng tất cả các phân phối, nhưng điều đó vẫn ngầm khám phá sự sắp xếp theo cấp số nhân. 

Trường hợp cạnh xuất hiện khi`m = 0`hoặc`m = n`. Trong những trường hợp đó, trình tự được cố định: tất cả KHÔNG HOÀN HẢO hoặc tất cả HOÀN HẢO tương ứng. Bất kỳ giải pháp nào cũng phải nhận ra ngay các cấu hình suy biến này, nếu không sẽ có nguy cơ tính toán không cần thiết hoặc lý luận không chính xác về các lần chạy trống. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu từ việc tạo ra mọi chuỗi nhị phân có độ dài`n`với chính xác`m`những cái (HOÀN HẢO). Đối với mỗi cấu hình, chúng tôi tính toán thời gian chạy liên tiếp dài nhất trong`O(n)`thời gian. Vì có`C(n, m)`những cấu hình như vậy, tổng độ phức tạp là vào thứ tự`O(C(n, m) * n)`, điều này vượt xa khả thi ngay cả đối với những đầu vào nhỏ như`n = 1000, m = 500`. 

Quan sát quan trọng là điểm số chỉ phụ thuộc vào cách chúng ta chia`m`các mục giống nhau thành các nhóm liền kề. KHÔNG HOÀN HẢO đóng vai trò là dấu phân cách cho phép chúng ta ngắt các lần chạy HOÀN HẢO. Vì vậy, vấn đề trở thành: đưa ra`m`các đối tượng giống hệt nhau, nhóm lớn nhất có thể lớn hay nhỏ như thế nào nếu chúng ta được phép phân phối chúng trên các phân khúc? 

Để đạt được số điểm tối đa, chúng tôi muốn tránh việc chia các câu HOÀN THÀNH. Nếu chúng ta đặt tất cả`m`HOÀN THÀNH liên tiếp, chúng ta thu được một khối có độ dài duy nhất`m`. Điều này rõ ràng là tối ưu vì bất kỳ KHÔNG HOÀN HẢO nào được chèn vào bên trong sẽ chia tách khối và không thể tăng thời lượng chạy tối đa. 

Để đạt được điểm tối thiểu, chúng tôi muốn trải rộng các HOÀN THÀNH một cách đồng đều nhất có thể để không có khối liền kề nào trở nên quá lớn. Nếu chúng ta chia`m`mục vào`n - m + 1`các vị trí có thể có (khoảng trống giữa các phần KHÔNG HOÀN HẢO, bao gồm cả phần cuối), chúng tôi muốn phân bổ chúng một cách đồng đều nhất có thể trên các vị trí này. Tải tối đa của bất kỳ vị trí nào sau khi cân bằng tối ưu sẽ xác định thời gian chạy liên tiếp dài nhất có thể tối thiểu. 

Điều này rút gọn thành cách giải thích cân bằng tải cổ điển: chúng tôi đặt`m`những quả bóng không thể phân biệt được thành`k = n - m + 1`xô, giảm thiểu kích thước thùng tối đa. Câu trả lời là trần nhà`m / k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n, m) · n) | O(n) | Quá chậm | 
| Tối ưu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc`n`Và`m`. Những điều này xác định số lượng phần tử HOÀN HẢO và KHÔNG HOÀN HẢO mà chúng ta phải đặt. 
2. Tính số điểm tối đa có thể. Vì việc nhóm tất cả các HOÀN THÀNH lại với nhau luôn hợp lệ nên khối liền kề lớn nhất mà chúng ta có thể hình thành chỉ đơn giản là`m`. 
3. Tính số lượng khe phân cách có sẵn như sau:`k = n - m + 1`. Điều này tương ứng với số lượng phần HOÀN THÀNH có thể tồn tại khi chúng ta xen kẽ những phần KHÔNG HOÀN THÀNH một cách tối ưu. Bước này thể hiện ý tưởng rằng mọi KHÔNG HOÀN HẢO đều có khả năng phân chia một khối, làm tăng sự phân mảnh. 
4. Nếu`m = 0`, cả điểm tối đa và tối thiểu đều là`0`, vì không có HOÀN HẢO nào cả. 
5. Nếu`m > 0`, tính kích thước khối tối đa tối thiểu có thể bằng cách phân phối`m`các mục càng đồng đều càng tốt trên`k`khe cắm. Tải tối đa nhỏ nhất có thể đạt được là`(m + k - 1) // k`. 
6. Xuất cặp`(smax, smin)`. 

Bước suy luận chính là sự chuyển đổi từ sắp xếp trình tự sang phân phối trên các vị trí. Sau khi ánh xạ này được thực hiện, câu trả lời sẽ trở thành một phép tính số học đơn giản. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi hợp lệ nào cũng có thể được coi là các khối HOÀN THÀNH xen kẽ được phân tách bằng ít nhất một KHÔNG HOÀN THÀNH, ngoại trừ có thể ở cuối. Điều này tạo ra nhiều nhất`n - m + 1`các khe có thể đặt các khối HOÀN HẢO. Lần chạy HOÀN HẢO liền kề dài nhất chính xác là số lượng HOÀN THÀNH lớn nhất được gán cho bất kỳ vị trí nào. Việc tối đa hóa hoặc giảm thiểu mức tối đa đó sẽ giảm xuống mức thu gọn mọi thứ vào một khe hoặc phân phối vật phẩm đồng đều nhất có thể. Không có sự sắp xếp nào bên ngoài mô hình này có thể thay đổi giới hạn cơ bản được áp đặt bằng cách chia tách thông qua KHÔNG HOÀN THÀNH. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())

        if m == 0:
            print(0, 0)
            continue

        smax = m

        k = n - m + 1
        smin = (m + k - 1) // k

        print(smax, smin)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo các công thức dẫn xuất. Trường hợp tế nhị duy nhất là`m = 0`, trong đó công thức chung vẫn hoạt động nhưng làm cho lý do kém minh bạch hơn nên nó được xử lý một cách rõ ràng. 

giá trị`smax = m`xuất phát từ quan sát rằng việc hợp nhất tất cả các HOÀN THÀNH vào một phân đoạn liền kề không bao giờ vi phạm các ràng buộc. Việc tính toán của`k`phản ánh có bao nhiêu phân đoạn HOÀN THÀNH rời rạc có thể tồn tại nếu chúng ta chèn tất cả các phân đoạn KHÔNG HOÀN THÀNH làm dấu phân cách. Bộ phận trần tính toán giới hạn chặt chẽ nhất có thể khi phân phối đồng đều các phần tử giống nhau. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1:`n = 5, m = 4`Chúng ta có 4 cái HOÀN HẢO và 1 cái KHÔNG HOÀN HẢO. 

| Bước | m | k = n-m+1 | smax | smin | 
| --- | --- | --- | --- | --- | 
| ban đầu | 4 | 2 | - | - | 
| tính toán tối đa | - | - | 4 | - | 
| tính toán tối thiểu | - | - | 4 | (4+2-1)//2 = 2 | 

Sự sắp xếp tối đa là`PPPPN`, cho kết quả là 4. Đạt được mức tối thiểu bằng cách tách một từ KHÔNG HOÀN THÀNH thành các từ HOÀN THÀNH, chẳng hạn như`PPNPP`, tạo ra các chuỗi có độ dài 2. 

Điều này xác nhận rằng KHÔNG HOÀN HẢO đóng vai trò như một dấu phân cách buộc phải chia tách khi có thể. 

### Ví dụ 2:`n = 10, m = 3`| Bước | m | k = n-m+1 | smax | smin | 
| --- | --- | --- | --- | --- | 
| ban đầu | 3 | 8 | - | - | 
| tính toán tối đa | - | - | 3 | - | 
| tính toán tối thiểu | - | - | 3 | (3+8-1)//8 = 1 | 

Ở đây có đủ các vị trí để chúng ta có thể tách riêng từng HOÀN THÀNH riêng lẻ, do đó thời gian chạy dài nhất tối thiểu trở thành 1. Cấu trúc cho phép phân tách hoàn toàn, phù hợp với việc phân phối các mục thưa thớt trên nhiều khoảng trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi lần kiểm tra | Chỉ các phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Với`T ≤ 100`, thao tác này sẽ chạy ngay lập tức. Giải pháp này tránh mọi phép liệt kê tổ hợp và giảm vấn đề về lý luận theo thời gian không đổi cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())

        if m == 0:
            out.append("0 0")
            continue

        smax = m
        k = n - m + 1
        smin = (m + k - 1) // k
        out.append(f"{smax} {smin}")

    return "\n".join(out)

# provided samples
assert run("5\n5 4\n100 50\n252 52\n3 0\n10 10\n") == "4 2\n50 1\n52 1\n0 0\n10 10"

# custom cases
assert run("1\n1 0\n") == "0 0"
assert run("1\n1 1\n") == "1 1"
assert run("1\n6 1\n") == "1 1"
assert run("1\n6 5\n") == "5 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 0 0 | bộ HOÀN HẢO trống | 
| 1 1 | 1 1 | trình tự điền đầy đủ | 
| 6 1 | 1 1 | HOÀN HẢO duy nhất hành xử tầm thường | 
| 6 5 | 5 3 | phân phối chia không tầm thường | 

## Vỏ cạnh 

Khi nào`m = 0`, không có HOÀN THÀNH nào cả, vì vậy không có phân đoạn liền kề nào có thể tồn tại. Thuật toán trả về một cách rõ ràng`0 0`, phù hợp với định nghĩa của một mức tối đa trống. 

Khi`m = n`, tất cả các yếu tố đều HOÀN HẢO. Công thức cho`smax = n`, Và`k = 1`, Vì thế`smin = n`. Thuật toán trả về chính xác một cấu hình bắt buộc duy nhất. 

Khi`m = 1`, bất kể`n`, chỉ có một HOÀN THÀNH, vì vậy cả tối đa và tối thiểu đều bằng 1. Công thức tạo ra`(1 + (n - 1 + 1) - 1) // (n - 1 + 1) = 1`, phù hợp với sự cô lập của một phần tử. 

Những trường hợp này xác nhận rằng cách giải thích dựa trên vị trí vẫn hợp lệ ở mật độ cực cao, trong đó không thể phân tách hoặc phân tách là không đáng kể.
