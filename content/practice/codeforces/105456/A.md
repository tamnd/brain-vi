---
title: "CF 105456A - Palindrome ABC"
description: "Chúng ta được đếm ba ký tự, cụ thể là chúng ta phải sử dụng các chữ cái A, B và C bao nhiêu lần. Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là xây dựng một chuỗi bằng cách sử dụng chính xác các ký tự đó sao cho chuỗi đọc xuôi và ngược giống nhau, và trong số tất cả các chuỗi hợp lệ như vậy…"
date: "2026-06-23T17:44:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105456
codeforces_index: "A"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Day 2"
rating: 0
weight: 105456
solve_time_s: 65
verified: true
draft: false
---

[CF 105456A - Palindrome ABC](https://codeforces.com/problemset/problem/105456/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đếm ba ký tự, cụ thể là chúng ta phải sử dụng các chữ cái A, B và C bao nhiêu lần. Đối với mỗi trường hợp thử nghiệm, nhiệm vụ là xây dựng một chuỗi sử dụng chính xác các ký tự đó sao cho chuỗi đó đọc xuôi và ngược giống nhau, và trong số tất cả các chuỗi hợp lệ như vậy, chúng ta phải xuất ra chuỗi nhỏ nhất theo thứ tự từ điển. Nếu không có bảng màu nào có thể được tạo bằng cách sử dụng tất cả các ký tự thì câu trả lời là “ROP”. 

Một palindrome áp đặt một ràng buộc cấu trúc nghiêm ngặt: các ký tự ở vị trí đối xứng phải khớp nhau. Điều này ngay lập tức có nghĩa là việc phân bổ tần số không phải là tùy ý. Nhiều nhất một loại ký tự có thể xuất hiện với số lần lẻ, bởi vì một palindrome có nhiều nhất một ký tự trung tâm mà không có đối tác phản chiếu. 

Các ràng buộc đủ nhỏ để mỗi trường hợp thử nghiệm chỉ yêu cầu công việc không đổi hoặc tuyến tính về kích thước bảng chữ cái. Vì tổng số ký tự cho mỗi trường hợp thử nghiệm nhiều nhất là 3000 và có tới 1000 trường hợp thử nghiệm nên mọi cách tiếp cận xây dựng chuỗi trực tiếp đều dễ dàng đủ nhanh. Điều chúng tôi không thể làm được là thử tất cả các hoán vị hoặc thực hiện các công trình xây dựng kiểu giai thừa. 

Một dạng lỗi phổ biến là bỏ qua tính chẵn lẻ. Ví dụ: nếu chúng ta cố gắng xây dựng một palindrome một cách tham lam mà không kiểm tra tính khả thi trước, thì cuối cùng chúng ta có thể tạo ra một nửa chuỗi không thể phản chiếu một cách nhất quán. 

Trường hợp cạnh tinh tế thứ hai phát sinh khi tồn tại nhiều số lẻ. Ví dụ: đầu vào như 1 1 1 có ba số lẻ. Không có cách nào để đặt nhiều hơn một ký tự lẻ vào giữa nên không có giải pháp nào tồn tại. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là tạo ra tất cả các hoán vị của nhiều tập hợp các chữ cái và lọc những chữ cái đó là các bảng màu, sau đó chọn cái nhỏ nhất về mặt từ điển. Điều này rất đơn giản về mặt khái niệm: chúng tôi liệt kê mọi cách sắp xếp của A, B và C với số lượng đã cho, kiểm tra xem đó có phải là một bảng màu hay không và theo dõi kết quả từ điển nhỏ nhất. 

Vấn đề là sự bùng nổ tổ hợp. Nếu tổng chiều dài là N thì số hoán vị riêng biệt là N! chia cho các giai thừa lặp lại. Ngay cả đối với N vừa phải như 30, điều này hoàn toàn không khả thi. Kiểm tra palindrome là O(N), do đó độ phức tạp tổng thể trở thành thang đo giai thừa. 

Quan sát quan trọng là một palindrome hoàn toàn được xác định bởi nửa đầu của nó cộng với có thể là ký tự ở giữa. Sau khi chúng tôi quyết định có bao nhiêu chữ cái được đưa vào nửa bên trái, nửa bên phải sẽ bị ép buộc. Điều này biến bài toán từ tìm kiếm hoán vị thành bài toán đếm có ràng buộc. 

Chúng ta chỉ cần quyết định xem có bao nhiêu A, B và C ở vế trái sao cho các số còn lại có thể phản chiếu chính xác. Điều đó có nghĩa là đối với mỗi ký tự, số được sử dụng ở cả hai bên phải là số chẵn và mọi ký tự lẻ còn sót lại (nhiều nhất là một loại) sẽ được đưa vào giữa. Sau khi xây dựng nửa bên trái, tính tối thiểu về mặt từ điển đạt được bằng cách điền nó một cách tham lam từ ký tự nhỏ nhất đến ký tự lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(N!) | O(N) | Quá chậm | 
| Xây dựng từ một nửa số lượng | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giảm vấn đề xuống còn việc quyết định tính khả thi của một bảng màu và sau đó xây dựng bảng từ điển nhỏ nhất.

1. Đếm xem A, B, C có bao nhiêu số lẻ. Nếu có nhiều hơn một số lẻ thì không có bảng màu nào có thể tồn tại. Điều này là do mỗi palindrome có nhiều nhất một ký tự ở giữa không ghép đôi. 
2. Xác định ký tự ở giữa nếu nó tồn tại. Nếu chính xác một trong các số đếm là số lẻ thì chữ cái đó sẽ trở thành tâm của bảng màu. 
3. Chuyển tất cả số đếm thành nửa số bằng cách chia cho hai. Chúng đại diện cho số lượng mỗi ký tự sẽ xuất hiện ở phía bên trái của bảng màu. 
4. Xây dựng nửa bên trái của bảng màu bằng cách đặt các ký tự theo thứ tự từ điển, luôn sử dụng tất cả A có sẵn trước, sau đó là B, sau đó là C. Thứ tự tham lam này đảm bảo chuỗi tổng thể là nhỏ nhất về mặt từ điển vì các vị trí trước đó chiếm ưu thế so sánh từ điển. 
5. Xây dựng đáp án cuối cùng là nửa trái + nửa giữa (nếu có) + nửa trái ngược. 

Tính chính xác phụ thuộc vào thực tế là sau khi cố định một nửa số đếm, thứ tự bên trong nửa bên trái sẽ trực tiếp xác định thứ tự từ điển của bảng màu đầy đủ. 

### Tại sao nó hoạt động 

Bất kỳ palindrome hợp lệ nào cũng được xác định duy nhất bởi nửa bên trái và ký tự trung tâm tùy chọn của nó. Ràng buộc khả thi chỉ hạn chế tính chẵn lẻ và không sắp xếp thứ tự. Vì so sánh từ điển được quyết định từ trái sang phải, nên việc giảm thiểu nửa bên trái một cách tham lam theo thứ tự bảng chữ cái sẽ đảm bảo chuỗi đầy đủ nhỏ nhất có thể. Không có sự sắp xếp lại nào của nửa bên phải có thể thay đổi thứ tự này vì nó hoàn toàn được xác định bởi tính đối xứng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        a, b, c = map(int, input().split())
        counts = [a, b, c]
        letters = ['A', 'B', 'C']
        
        odd = sum(x % 2 for x in counts)
        if odd > 1:
            out.append("ROP")
            continue
        
        mid = ""
        for i in range(3):
            if counts[i] % 2 == 1:
                mid = letters[i]
                counts[i] -= 1
                break
        
        half = [counts[i] // 2 for i in range(3)]
        
        left = []
        for i in range(3):
            left.append(letters[i] * half[i])
        
        left = "".join(left)
        right = left[::-1]
        
        out.append(left + mid + right)
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xác nhận liệu một palindrome có khả thi về mặt cấu trúc hay không bằng cách sử dụng tính chẵn lẻ. Sau đó, nó chỉ gán một ký tự ở giữa nếu cần, đảm bảo giảm chính xác một số lẻ. Một nửa số đếm thể hiện tính đối xứng bắt buộc và việc xây dựng phía bên trái theo thứ tự từ điển cố định đảm bảo tính tối thiểu. Cuối cùng, palindrome được hoàn thành bằng cách phản chiếu. 

Một điểm tinh tế là chúng tôi trừ đi một từ số lẻ trước khi giảm một nửa. Điều này đảm bảo rằng ký tự trung tâm không ảnh hưởng đến tính đối xứng. Một chi tiết quan trọng khác là tính tối ưu của từ điển không yêu cầu quay lui, vì tất cả các lựa chọn đều độc lập khi thứ tự của các nửa được cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 0 0
2 2 2
```Đối với mỗi trường hợp thử nghiệm: 

| Bước | Đếm (A,B,C) | Số lẻ | Trung | Một nửa tính | Trái | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1,0,0 | 1 | A | 0,0,0 | "" | 
| 2 | 2,2,2 | 3 | A/B/C không hợp lệ | - | - | 

Đối với trường hợp đầu tiên, tất cả các số đếm đều là số chẵn sau khi đặt A vào giữa, do đó kết quả là “A”. Đối với trường hợp thứ hai, tồn tại nhiều hơn một số lẻ, khiến cho việc hình thành bảng màu không thể thực hiện được, do đó đầu ra là “ROP”. 

Điều này xác nhận quy tắc chẵn lẻ phát hiện chính xác tính khả thi và việc xây dựng xử lý các bảng màu đơn chữ cái tầm thường. 

### Ví dụ 2 

đầu vào:```
1
2 0 3
```| Bước | Đếm | Số lẻ | Trung | Một nửa tính | Trái | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2,0,3 | 1 | C | 1,0,1 | "AC" | 

Chúng ta đặt C ở giữa, giảm số đếm của nó xuống còn 2 và lấy một nửa: A đóng góp 1, C đóng góp 1. Nửa bên trái trở thành “AC”, và bảng màu cuối cùng là “ACCCA”. 

Điều này cho thấy cách thuật toán tách biệt một cách tự nhiên việc xây dựng đối xứng khỏi việc xử lý trung tâm mà không có sự mơ hồ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm xử lý kích thước bảng chữ cái không đổi và xây dựng tối đa 1000 ký tự | 
| Không gian | O(1) | Chỉ các mảng cố định có kích thước 3 cộng với bộ nhớ đầu ra | 

Các phép toán trên mỗi trường hợp thử nghiệm là số học theo thời gian không đổi và nối chuỗi được giới hạn bởi tổng chiều dài, vốn nhỏ. Điều này phù hợp thoải mái trong giới hạn ngay cả đối với số lượng trường hợp thử nghiệm tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            a, b, c = map(int, input().split())
            counts = [a, b, c]
            letters = ['A', 'B', 'C']
            
            odd = sum(x % 2 for x in counts)
            if odd > 1:
                out.append("ROP")
                continue
            
            mid = ""
            for i in range(3):
                if counts[i] % 2 == 1:
                    mid = letters[i]
                    counts[i] -= 1
                    break
            
            half = [counts[i] // 2 for i in range(3)]
            
            left = []
            for i in range(3):
                left.append(letters[i] * half[i])
            
            left = "".join(left)
            out.append(left + mid + left[::-1])
        
        return "\n".join(out)
    
    return solve()

# provided samples
assert run("4\n1 0 0\n2 2 2\n2 0 3\n1 3 5\n") == "A\nABCCBA\nACCCA\nROP"

# all even
assert run("2\n2 2 0\n4 0 2\n") == "ABBA\nAACCAA"

# all zeros except one
assert run("1\n0 0 5\n") == "CCCCC"

# impossible case
assert run("1\n1 1 1\n") == "ROP"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 0 / 4 0 2 | ABBA / AACCAA | xây dựng đồng đều cơ bản | 
| 0 0 5 | CCCCC | xử lý trung tâm bảng chữ cái đơn | 
| 1 1 1 | ROP | từ chối nhiều số lẻ | 

## Vỏ cạnh 

Đối với đầu vào`0 0 5`, thuật toán phát hiện một số lẻ (C), gán nó vào giữa, giảm xuống còn 4 và xây dựng một nửa số đếm (0,0,2). Nửa bên trái trở thành “CC”, tạo ra “CCCCC”. Điều này xác nhận việc xử lý chính xác sự thống trị thuần túy của một ký tự. 

Đối với đầu vào`1 1 1`, cả ba số đếm đều là số lẻ, do đó thuật toán ngay lập tức trả về “ROP” trước khi thử xây dựng. Điều này cho thấy việc kiểm tra tính khả thi sẽ ngăn chặn việc sắp xếp bắt buộc không chính xác. 

Đối với đầu vào`2 0 3`, thuật toán gán C vào giữa, để lại các nửa cân bằng. Cấu trúc “AC + C + CA” đồng thời khẳng định tính đối xứng và tính chính xác về mặt từ điển.
