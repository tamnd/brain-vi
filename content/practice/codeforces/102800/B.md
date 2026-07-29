---
title: "CF 102800B - Lựa chọn vấn đề"
description: "Nhiệm vụ là lựa chọn các vấn đề cho một cuộc thi. Mỗi vấn đề được xác định bằng một URL, nhưng thông tin hữu ích được ẩn bên trong URL: ID vấn đề số nguyên ở cuối."
date: "2026-07-28T22:52:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102800
codeforces_index: "B"
codeforces_contest_name: "The 14th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 102800
solve_time_s: 56
verified: true
draft: false
---

[CF 102800B - Lựa chọn vấn đề](https://codeforces.com/problemset/problem/102800/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là lựa chọn các vấn đề cho một cuộc thi. Mỗi vấn đề được xác định bằng một URL, nhưng thông tin hữu ích được ẩn bên trong URL: ID vấn đề số nguyên ở cuối. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được một tập hợp các URL có vấn đề duy nhất và phải trích xuất ID của chúng, sắp xếp chúng và in phần nhỏ nhất.`k`ID theo thứ tự tăng dần 

Số lượng URL trong trường hợp thử nghiệm nhiều nhất là 1000. Con số này đủ nhỏ để ngay cả cách tiếp cận sắp xếp đơn giản cũng có thể đủ nhanh. Khác biệt`O(n log n)`sắp xếp chỉ thực hiện khoảng vài nghìn so sánh cho`n = 1000`, do đó không cần cấu trúc dữ liệu phức tạp hơn. Phạm vi ID cũng bị giới hạn từ 1 đến 10000, có nghĩa là ngay cả các phương pháp dựa trên việc đếm cũng có thể thực hiện được, nhưng chúng không cần thiết. 

Kích thước đầu vào vẫn yêu cầu phân tích cú pháp cẩn thận vì URL là một chuỗi chứ không phải là số. Một lỗi phổ biến là cho rằng ID luôn có số chữ số cố định. Ví dụ: URL kết thúc bằng`501`và URL kết thúc bằng`1001`cả hai đều hợp lệ, do đó giải pháp phải trích xuất mọi thứ sau dấu gạch chéo cuối cùng. 

Một trường hợp cạnh khác là khi`k`bằng với`n`. Trong tình huống này, câu trả lời là toàn bộ bộ ID đã được sắp xếp. Giải pháp chỉ xử lý một phần dữ liệu có thể vô tình bỏ sót các giá trị. 

Ví dụ, hãy xem xét:```
1
3 3
http://acm.hit.edu.cn/problemset/9
http://acm.hit.edu.cn/problemset/2
http://acm.hit.edu.cn/problemset/7
```Đầu ra đúng là:```
2 7 9
```Việc thực hiện bất cẩn giả định`k`luôn nhỏ hơn`n`có thể không xuất ra tất cả các giá trị. 

Một trường hợp khác là khi ID chứa ít chữ số hơn các ID khác:```
1
4 2
http://acm.hit.edu.cn/problemset/501
http://acm.hit.edu.cn/problemset/50
http://acm.hit.edu.cn/problemset/500
http://acm.hit.edu.cn/problemset/1000
```Đầu ra đúng là:```
50 500
```Việc sắp xếp trực tiếp các chuỗi URL sẽ đưa ra thứ tự sai vì so sánh chuỗi không khớp với so sánh số nguyên. Giải pháp phải chuyển đổi văn bản được trích xuất thành số nguyên trước khi sắp xếp. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là đọc mọi URL, trích xuất ID, lưu trữ tất cả ID trong một mảng, sắp xếp mảng và lấy ID đầu tiên.`k`các phần tử. Điều này có tác dụng vì câu trả lời bắt buộc chỉ phụ thuộc vào thứ tự số của ID. Sau khi tất cả các ID được sắp xếp, giá trị nhỏ nhất`k`giá trị chính xác là giá trị đầu tiên`k`các vị trí. 

Một giải pháp thay thế mạnh mẽ sẽ liên tục tìm kiếm ID nhỏ nhất chưa được sử dụng. Đối với mỗi`k`câu trả lời được yêu cầu, nó sẽ quét tất cả các ID còn lại để tìm mức tối thiểu tiếp theo. Điều này đúng, nhưng trong trường hợp xấu nhất nó thực hiện khoảng`n * k`so sánh. Từ`k`có thể bằng`n`, trường hợp xấu nhất đạt đến`1000 * 1000 = 1,000,000`so sánh cho mỗi trường hợp thử nghiệm. Điều này vẫn có thể chấp nhận được đối với những ràng buộc này, nhưng nó bỏ qua cấu trúc tiêu chuẩn của vấn đề và trở thành thói quen xấu đối với các phiên bản lớn hơn. 

Quan sát tốt hơn là bài toán này chính xác đang yêu cầu các phần tử nhỏ nhất trong một tập hợp số. Khi các URL đã được chuyển đổi thành ID, không còn cấu trúc đặc biệt nào nữa. Việc sắp xếp đưa ra thứ tự hoàn chỉnh trong một thao tác và thao tác đầu tiên`k`các yếu tố là câu trả lời mong muốn. 

Phương pháp brute-force hoạt động vì nó mô phỏng trực tiếp việc chọn phần tử nhỏ nhất tiếp theo, nhưng nó lặp lại các tìm kiếm tương tự. Quan sát sắp xếp sẽ loại bỏ công việc lặp đi lặp lại đó bằng cách sắp xếp tất cả các giá trị cùng một lúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk), trường hợp xấu nhất O(n²) | O(n) | Được chấp nhận ở đây, nhưng không hiệu quả | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`Và`k`, sau đó chuẩn bị danh sách để lưu trữ ID vấn đề được trích xuất. Bản thân URL không cần thiết sau khi lấy được ID số của nó. 
2. Xử lý từng`n`URL. Tìm phần sau trận chung kết`/`ký tự và chuyển nó thành số nguyên. Thành phần cuối cùng của URL được đảm bảo là ID sự cố, vì vậy việc trích xuất này mang lại chính xác giá trị mà chúng ta cần. 
3. Sắp xếp danh sách ID theo thứ tự tăng dần. Sắp xếp là thao tác quan trọng vì nó đặt các ID nhỏ nhất ở đầu danh sách. 
4. Xuất đầu tiên`k`các giá trị từ danh sách được sắp xếp, cách nhau bằng dấu cách. Vì danh sách đã được sắp xếp sẵn nên không cần xử lý thêm. 

Tại sao nó hoạt động: 

Sau khi trích xuất, thuật toán có bộ số giống hệt như các bài toán ban đầu, chỉ được biểu diễn dưới dạng số nguyên thay vì chuỗi. Việc sắp xếp bảo toàn mọi giá trị trong khi sắp xếp chúng từ nhỏ nhất đến lớn nhất. đầu tiên`k`các vị trí trong thứ tự này phải chứa`k`ID nhỏ nhất vì mọi vị trí còn lại đều chứa một giá trị ít nhất bằng giá trị được chọn. Do đó, đầu ra được tạo ra luôn là bộ ID vấn đề được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())
        ids = []

        for _ in range(n):
            url = input().strip()
            ids.append(int(url.rsplit('/', 1)[1]))

        ids.sort()
        ans.append(" ".join(map(str, ids[:k])))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu tiên, giải pháp sẽ đọc số lượng trường hợp thử nghiệm và xử lý từng trường hợp một cách độc lập. Danh sách`ids`chỉ lưu trữ số nguyên vì định dạng URL không liên quan sau khi ID được trích xuất. 

biểu thức`url.rsplit('/', 1)[1]`chỉ tách chuỗi ở dấu gạch chéo cuối cùng. Điều này tránh việc phụ thuộc vào cấu trúc chính xác của phần trước của URL. Nó xử lý chính xác các ID có độ dài chữ số khác nhau, chẳng hạn như`9`,`501`, Và`10000`. 

Sau khi tất cả ID được thu thập,`sort()`sắp xếp chúng theo số lượng. Python so sánh các số nguyên theo giá trị, đây là thứ tự mà bài toán yêu cầu. Cuối cùng, cắt bằng`ids[:k]`chọn chính xác số lượng ID nhỏ nhất được yêu cầu. Từ`k`có thể bằng`n`, điều này cũng xử lý chính xác trường hợp phải in toàn bộ danh sách đã sắp xếp. 

Sản lượng được tích lũy trong`ans`và in một lần ở cuối. Điều này tránh việc xả nước không cần thiết và giữ đầu vào và đầu ra hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
3 2
http://acm.hit.edu.cn/problemset/1003
http://acm.hit.edu.cn/problemset/1002
http://acm.hit.edu.cn/problemset/1001
```Dấu vết: 

| Bước | ID được trích xuất | ID được sắp xếp | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc URL đầu tiên | 1003 | | | 
| Đọc URL thứ hai | 1003, 1002 | | | 
| Đọc URL thứ ba | 1003, 1002, 1001 | | | 
| Sắp xếp | 1003, 1002, 1001 | 1001, 1002, 1003 | | 
| Lấy 2 đầu tiên | | 1001, 1002, 1003 | 1001 1002 | 

Dấu vết cho thấy thứ tự URL ban đầu không có hiệu lực. Chỉ các ID số được trích xuất mới quan trọng sau khi phân tích cú pháp. 

### Mẫu 2 

đầu vào:```
1
4 1
http://acm.hit.edu.cn/problemset/1001
http://acm.hit.edu.cn/problemset/2001
http://acm.hit.edu.cn/problemset/3001
http://acm.hit.edu.cn/problemset/501
```Dấu vết: 

| Bước | ID được trích xuất | ID được sắp xếp | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc URL | 1001, 2001, 3001, 501 | | | 
| Sắp xếp | | 501, 1001, 2001, 3001 | | 
| Lấy 1 đầu tiên | | 501, 1001, 2001, 3001 | 501 | 

Ví dụ này chứng tỏ tại sao việc chuyển đổi số nguyên lại quan trọng. Nếu ID được so sánh dưới dạng chuỗi,`1001`có thể xuất hiện không chính xác trước đó`501`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc trích xuất ID mất O(n) và việc sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Danh sách lưu trữ tất cả các ID được trích xuất cho một trường hợp thử nghiệm | 

Tối đa`n`chỉ có 1000, vì vậy việc sắp xếp dễ dàng trong thời gian giới hạn. Việc sử dụng bộ nhớ cũng nhỏ vì chỉ cần lưu trữ ID số. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    def main():
        input = sys.stdin.readline
        t = int(input())
        ans = []

        for _ in range(t):
            n, k = map(int, input().split())
            ids = []
            for _ in range(n):
                url = input().strip()
                ids.append(int(url.rsplit('/', 1)[1]))
            ids.sort()
            ans.append(" ".join(map(str, ids[:k])))

        print("\n".join(ans), end="")

    main()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return output.getvalue()

assert solve("""3
3 2
http://acm.hit.edu.cn/problemset/1003
http://acm.hit.edu.cn/problemset/1002
http://acm.hit.edu.cn/problemset/1001
4 1
http://acm.hit.edu.cn/problemset/1001
http://acm.hit.edu.cn/problemset/2001
http://acm.hit.edu.cn/problemset/3001
http://acm.hit.edu.cn/problemset/501
1 3
http://acm.hit.edu.cn/problemset/9
http://acm.hit.edu.cn/problemset/2
http://acm.hit.edu.cn/problemset/7
""") == """1001 1002
501
2 7 9""", "samples"

assert solve("""1
1 1
http://acm.hit.edu.cn/problemset/10000
""") == "10000", "minimum size case"

assert solve("""1
5 5
http://acm.hit.edu.cn/problemset/5
http://acm.hit.edu.cn/problemset/4
http://acm.hit.edu.cn/problemset/3
http://acm.hit.edu.cn/problemset/2
http://acm.hit.edu.cn/problemset/1
""") == "1 2 3 4 5", "k equals n"

assert solve("""1
4 2
http://acm.hit.edu.cn/problemset/50
http://acm.hit.edu.cn/problemset/500
http://acm.hit.edu.cn/problemset/501
http://acm.hit.edu.cn/problemset/1000
""") == "50 500", "different digit lengths"

assert solve("""1
6 3
http://acm.hit.edu.cn/problemset/8
http://acm.hit.edu.cn/problemset/8
http://acm.hit.edu.cn/problemset/8
http://acm.hit.edu.cn/problemset/8
http://acm.hit.edu.cn/problemset/8
http://acm.hit.edu.cn/problemset/8
""") == "8 8 8", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cung cấp mẫu |`1001 1002`,`501`,`2 7 9`| Trích xuất, sắp xếp và lựa chọn cơ bản | 
| URL đơn |`10000`| Kích thước đầu vào tối thiểu và ID ranh giới lớn | 
|`k = n`|`1 2 3 4 5`| In danh sách đã sắp xếp hoàn chỉnh | 
| Độ dài chữ số hỗn hợp |`50 500`| Sắp xếp số nguyên thay vì sắp xếp chuỗi | 
| Tất cả các giá trị bằng nhau |`8 8 8`| Xử lý các giá trị trùng lặp trong triển khai tổng quát | 

## Vỏ cạnh 

Khi nào`k`bằng số lượng URL, thuật toán không cần nhánh đặc biệt. Đối với đầu vào:```
1
3 3
http://acm.hit.edu.cn/problemset/9
http://acm.hit.edu.cn/problemset/2
http://acm.hit.edu.cn/problemset/7
```danh sách trích xuất là`[9, 2, 7]`. Sắp xếp mang lại`[2, 7, 9]`, và cắt bằng`[:3]`trả về mọi phần tử. Đầu ra là:```
2 7 9
```Một giải pháp giả định rằng sẽ luôn có ít giá trị được yêu cầu hơn có thể dừng sớm do nhầm lẫn. 

Đối với các ID có số chữ số khác nhau, thuật toán sẽ chuyển văn bản thành số nguyên trước khi sắp xếp. Với:```
1
4 2
http://acm.hit.edu.cn/problemset/501
http://acm.hit.edu.cn/problemset/50
http://acm.hit.edu.cn/problemset/500
http://acm.hit.edu.cn/problemset/1000
```các giá trị được trích xuất là`[501, 50, 500, 1000]`. Sắp xếp theo số lượng tạo ra`[50, 500, 501, 1000]`, vì vậy hai giá trị đầu tiên được in:```
50 500
```Giải pháp dựa trên chuỗi sẽ so sánh các ký tự và có thể đặt các giá trị sai thứ tự. 

Khi tất cả các ID đều bằng nhau, thao tác sắp xếp vẫn hoạt động chính xác. Mặc dù vấn đề ban đầu đảm bảo các ID duy nhất, nhưng việc kiểm tra trường hợp này sẽ xác minh rằng bản thân logic lựa chọn không phụ thuộc vào tính duy nhất. Đối với sáu URL chứa ID`8`Và`k = 3`, sắp xếp để lại các giá trị như`[8, 8, 8, 8, 8, 8]`và câu trả lời là ba giá trị đầu tiên:```
8 8 8
```
