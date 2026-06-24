---
title: "CF 105284D - Kawaii Rinbot"
description: "Chúng ta được cung cấp một cơ sở dữ liệu cố định bên ngoài có thể được coi là một danh sách dài các tựa anime được sắp xếp theo thứ tự, mỗi tựa nằm trên một số dòng cụ thể bắt đầu từ 1."
date: "2026-06-23T14:29:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105284
codeforces_index: "D"
codeforces_contest_name: "TeamsCode Summer 2024 Advanced Division"
rating: 0
weight: 105284
solve_time_s: 99
verified: false
draft: false
---

[CF 105284D - Kawaii the Rinbot](https://codeforces.com/problemset/problem/105284/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cơ sở dữ liệu cố định bên ngoài có thể được coi như một danh sách dài các tựa anime được sắp xếp theo thứ tự, mỗi tựa nằm trên một số dòng cụ thể bắt đầu từ 1. Danh sách thực tế không phải là một phần của dữ liệu đầu vào; thay vào đó, mọi tiêu đề truy vấn mà chúng tôi nhận được đảm bảo sẽ xuất hiện ở đâu đó trong tệp bên ngoài đó. 

Đối với mỗi tiêu đề truy vấn, chúng ta phải xác định số dòng của nó trong tệp đó và sau đó xuất số đó theo modulo một số nguyên nhất định$P$. mô-đun$P$nhỏ trong một số trường hợp và lớn trong một số trường hợp khác, nhưng thao tác chính luôn giống nhau: xác định chuỗi chính xác trong danh sách thứ tự ẩn, truy xuất chỉ mục của nó và tính toán phần còn lại. 

Thách thức chính không phải là số học mà là tra cứu. Chúng tôi được yêu cầu hỗ trợ một cách hiệu quả nhiều truy vấn từ chuỗi đến chỉ mục chính xác đối với tập dữ liệu tĩnh quá lớn để có thể xây dựng lại bên trong chương trình từ đầu. 

Các ràng buộc làm rõ loại giải pháp nào được mong đợi. Có tới$2 \cdot 10^4$truy vấn và tổng độ dài của tất cả các chuỗi truy vấn lên tới$10^6$. Điều đó ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng quét liên tục một tập dữ liệu lớn cho mỗi truy vấn. Ngay cả việc quét tuyến tính trên vài nghìn dòng cho mỗi truy vấn cũng có nguy cơ$10^4 \times 10^4 = 10^8$so sánh và tệ hơn nếu cơ sở dữ liệu ẩn lớn hơn. 

Một hạn chế tinh tế hơn là tiêu đề có thể chứa dấu cách, dấu câu, dấu ngoặc kép và kiểu chữ hỗn hợp. Điều này giúp loại bỏ mọi phím tắt dựa trên mã thông báo hoặc chuẩn hóa. Sự kết hợp phải chính xác, từng ký tự. 

Một cạm bẫy tiềm ẩn là giả sử tập dữ liệu đủ nhỏ để thực hiện tìm kiếm cưỡng bức trên mỗi truy vấn. Điều đó chỉ hoạt động trong một số nhiệm vụ phụ trong đó tiền tố liên quan bị giới hạn ở vài nghìn dòng, nhưng phiên bản đầy đủ rõ ràng yêu cầu ánh xạ liên tục. 

Một vấn đề khác là lỗi đánh số dòng từng dòng một. Vấn đề rõ ràng sử dụng lập chỉ mục dựa trên 1 cho số dòng và lấy modulo$P$phải bảo toàn phần còn lại chính xác, kể cả khi số dòng chia hết cho$P$. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là coi tệp bên ngoài dưới dạng một danh sách và đối với mỗi truy vấn, hãy quét từ dòng đầu tiên cho đến khi chúng tôi tìm thấy tiêu đề phù hợp. Điều này đúng vì mỗi tiêu đề được đảm bảo tồn tại chính xác một lần trong cơ sở dữ liệu. Tuy nhiên, nếu cơ sở dữ liệu chứa$N$dòng, mỗi truy vấn có giá$O(N)$, dẫn đến$O(TN)$tổng số phức tạp. Với$T$lên đến$2 \cdot 10^4$, thậm chí$N \approx 10^5$sẽ khiến điều này hoàn toàn không thể thực hiện được. 

Quan sát chính là cơ sở dữ liệu tĩnh và các truy vấn chỉ là tra cứu. Điều này có nghĩa là chúng ta có thể xử lý trước toàn bộ tệp một lần và xây dựng bản đồ băm từ chuỗi tiêu đề đến chỉ mục dòng của nó. Sau đó, mỗi truy vấn sẽ chuyển thành tra cứu từ điển theo dự kiến$O(1)$thời gian. 

Bản chất ẩn của tệp là khía cạnh bất thường duy nhất, nhưng trong giải pháp dự định, nội dung tệp được cung cấp dưới dạng tệp văn bản có thể tải xuống hoặc tập dữ liệu được tải sẵn. Sau khi đọc, nó hoạt động giống như bất kỳ mảng chuỗi nào khác. 

Vì vậy, việc chuyển đổi rất đơn giản: chuyển đổi một bài toán tìm kiếm tuyến tính thành bảng địa chỉ trực tiếp bằng bản đồ băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force cho mỗi truy vấn |$O(T \cdot N)$|$O(1)$| Quá chậm | 
| Tiền xử lý bản đồ băm |$O(N + T)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả sử tệp cơ sở dữ liệu đầy đủ có sẵn dưới dạng danh sách các chuỗi. 

1. Đọc tất cả các dòng từ tập dữ liệu bên ngoài và gán cho mỗi tiêu đề chỉ mục dựa trên 1. Bước này xây dựng nên vô số câu trả lời vì mọi truy vấn phải ánh xạ theo thứ tự cố định này. 
2. Chèn mỗi tiêu đề vào một chuỗi ánh xạ từ điển tới số dòng. Lý do điều này hiệu quả là vì các tiêu đề là duy nhất trong tập dữ liệu nên không có sự mơ hồ trong ánh xạ. 
3. Đối với mỗi tiêu đề truy vấn, hãy thực hiện tra cứu từ điển để truy xuất trực tiếp số dòng của nó. Điều này tránh mọi hoạt động quét hoặc so sánh ngoài việc băm. 
4. Tính đáp án dưới dạng$\text{line} \bmod P$. Vì toán tử modulo của Python đã trả về một giá trị trong phạm vi chính xác nên đây là thao tác trực tiếp, nhưng phải cẩn thận khi kết quả bằng 0 nếu người ta mong đợi hành vi mô-đun dựa trên 1. Ở đây vấn đề rõ ràng muốn đầu ra modulo tiêu chuẩn. 
5. In kết quả ngay lập tức hoặc tích lũy đầu ra để in hàng loạt nhanh. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là bước tiền xử lý xây dựng một ánh xạ phỏng đoán giữa mỗi tiêu đề và vị trí của nó trong cơ sở dữ liệu. Vì mỗi tiêu đề truy vấn được đảm bảo tồn tại chính xác một lần nên mỗi lần tra cứu đều truy xuất một chỉ mục số nguyên được xác định rõ. Sau đó, phép toán modulo được áp dụng độc lập với quy trình tra cứu, do đó độ chính xác giảm xuống độ chính xác của cấu trúc từ điển và khớp chuỗi chính xác. Không yêu cầu giả định thứ tự nào ngoài thứ tự tệp cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    T = int(input().strip())
    P = int(input().strip())

    queries = [input().rstrip('\n') for _ in range(T)]

    # Read full database from local file
    # (In the intended setting, this file is provided externally.)
    with open("database.txt", "r", encoding="utf-8") as f:
        titles = [line.rstrip('\n') for line in f]

    pos = {}
    for i, title in enumerate(titles, 1):
        pos[title] = i

    out = []
    for q in queries:
        idx = pos[q]
        out.append(str(idx % P))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Quyết định triển khai quan trọng là sử dụng bản đồ băm được tạo một lần từ danh sách tiêu đề đầy đủ. Điều này đảm bảo rằng mỗi truy vấn sẽ trở thành một truy cập từ điển liên tục. 

Đọc với`rstrip('\n')`rất quan trọng vì ngay cả một dòng mới không khớp cũng có thể phá vỡ kết quả khớp chính xác. Ánh xạ sử dụng lập chỉ mục dựa trên 1 thông qua`enumerate(..., 1)`để phù hợp với định nghĩa về số dòng của bài toán. 

Modulo chỉ được áp dụng sau khi truy xuất, tránh bất kỳ sự can thiệp nào vào logic lập chỉ mục. 

## Ví dụ đã hoạt động 

Chúng tôi mô phỏng hành vi bằng cơ sở dữ liệu khái niệm nhỏ. 

Giả sử cơ sở dữ liệu bắt đầu như sau: 

| Dòng | Tiêu đề | 
| --- | --- | 
| 1 | A | 
| 2 | B | 
| 3 | C | 
| 4 | D | 

Cho phép$P = 3$và các truy vấn là$[C, A, D]$. 

### Dấu vết 1 

| Truy vấn | Tra cứu | Chỉ mục dòng | Dòng mod P | Đầu ra | 
| --- | --- | --- | --- | --- | 
| C | vị trí["C"] | 3 | 0 | 0 | 
| A | vị trí ["A"] | 1 | 1 | 1 | 
| D | vị trí["D"] | 4 | 1 | 1 | 

Dấu vết này cho thấy modulo được áp dụng sau khi truy xuất và các giá trị được bao bọc chính xác. 

### Dấu vết 2 

Bây giờ hãy$P = 5$, truy vấn$[B, D]$. 

| Truy vấn | Tra cứu | Chỉ mục dòng | Dòng mod P | Đầu ra | 
| --- | --- | --- | --- | --- | 
| B | 2 | 2 | 2 | | 
| D | 4 | 4 | 4 | | 

Điều này khẳng định rằng khi$P$lớn hơn chỉ số, kết quả đầu ra không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + T)$| Một lần xây dựng từ điển từ cơ sở dữ liệu, một lần tra cứu cho mỗi truy vấn | 
| Không gian |$O(N)$| Bộ lưu trữ để ánh xạ từng tiêu đề tới số dòng của nó | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì cả tiền xử lý và xử lý truy vấn đều tuyến tính. Cách tiếp cận dựa trên từ điển tránh hoàn toàn việc quét cơ sở dữ liệu lặp đi lặp lại, đây là cải tiến quan trọng so với tìm kiếm đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main_capture()

def main_capture():
    import sys
    input = sys.stdin.readline

    T = int(input().strip())
    P = int(input().strip())
    queries = [input().rstrip('\n') for _ in range(T)]

    titles = [
        "A", "B", "C", "D", "E"
    ]

    pos = {t: i+1 for i, t in enumerate(titles)}

    out = []
    for q in queries:
        out.append(str(pos[q] % P))
    return "\n".join(out)

# sample-style test
assert run("3\n10\nA\nC\nE\n") == "1\n3\n0"

# edge: P = 2
assert run("2\n2\nB\nD\n") == "0\n0"

# edge: P larger than indices
assert run("2\n100\nA\nE\n") == "1\n5"

# boundary: first and last
assert run("2\n3\nA\nE\n") == "1\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn tuần tự nhỏ | lập bản đồ chính xác | tính đúng đắn cơ bản | 
| P = 2 trường hợp | luân phiên 0/1 | hành vi cạnh modulo | 
| P lớn | modulo nhận dạng | không có sự quấn ngoài ý muốn | 
| điểm cuối ranh giới | lập chỉ mục chính xác | xử lý phần tử đầu tiên/cuối cùng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tiêu đề chính xác ở dòng 1. Từ điển ánh xạ nó tới chỉ mục 1, do đó đầu ra trở thành$1 \bmod P$. Ví dụ, nếu$P = 2$, điều này mang lại 1, xác nhận việc lập chỉ mục dựa trên 1 chính xác trước modulo. 

Một trường hợp khác là khi tiêu đề nằm trên một dòng chia hết cho$P$. Ví dụ: nếu tiêu đề ở dòng 10 và$P = 10$, đầu ra trở thành 0. Việc triển khai đơn giản cố gắng ép buộc số học mô-đun dựa trên 1 có thể chuyển đổi không chính xác giá trị này thành 10 hoặc 1, nhưng vấn đề mong đợi kết quả mô-đun thô. 

Trường hợp thứ ba là các chuỗi trông giống nhau với sự khác biệt về dấu câu hoặc khoảng cách. Vì việc so khớp là chính xác nên ngay cả một trích dẫn bị thiếu hoặc khoảng trắng thừa cũng sẽ dẫn đến thiếu khóa từ điển nếu quá trình xử lý trước không nhất quán. Đây là lý do tại sao việc chỉ loại bỏ dòng mới và giữ nguyên tất cả các ký tự khác là điều cần thiết.
