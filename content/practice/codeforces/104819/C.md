---
title: "CF 104819C - Tam giác"
description: "Chúng ta được cho một tập hợp các hình tam giác vuông giống hệt nhau có độ dài các cạnh là 3, 4 và 5. Mỗi hình tam giác là một ô cứng và chúng ta được phép đặt nhiều bản sao trên một mặt phẳng mà không chồng lên nhau."
date: "2026-06-28T13:00:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104819
codeforces_index: "C"
codeforces_contest_name: "2023 Sun Yat-sen University Collegiate Programming Contest, Onsite"
rating: 0
weight: 104819
solve_time_s: 52
verified: true
draft: false
---

[CF 104819C - Tam giác](https://codeforces.com/problemset/problem/104819/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các hình tam giác vuông giống hệt nhau có độ dài các cạnh là 3, 4 và 5. Mỗi hình tam giác là một ô cứng và chúng ta được phép đặt nhiều bản sao trên một mặt phẳng mà không chồng lên nhau. Mục đích là để xác định liệu có thể sắp xếp tất cả$n$các hình tam giác thành một hình duy nhất có trục đối xứng, nghĩa là sự kết hợp cuối cùng của tất cả các hình tam giác có thể được phản chiếu qua một số đường thẳng và khớp chính xác với nhau. 

Đầu vào chỉ bao gồm$n$, số lượng hình tam giác có sẵn. Chúng ta phải trả lời liệu có tồn tại một cách sắp xếp đối xứng hợp lệ sử dụng mọi tam giác chính xác một lần hay không. 

Ràng buộc$n \le 10^9$ngay lập tức loại bỏ bất kỳ cách tiếp cận nào cố gắng xây dựng hoặc mô phỏng hình học một cách rõ ràng. Bất kỳ giải pháp nào cũng phải phụ thuộc vào đặc tính cấu trúc về cách các hình tam giác này có thể kết hợp chứ không phải vào phép liệt kê. Quét tuyến tính, DP trên các vị trí hoặc bất kỳ cấu trúc hình học nào trên mỗi tam giác đều không khả thi vì ngay cả$O(n)$hoạt động đã quá lớn ở giới hạn trên. 

Một trường hợp cạnh tinh vi phát sinh từ những gì “hình dạng đối xứng trục” thực thi. Một tam giác đơn lẻ đã phá vỡ tính đối xứng phản chiếu vì tam giác vuông 3-4-5 không có đường thẳng nào ánh xạ nó lên chính nó. Vì vậy đối với$n = 1$, câu trả lời phải là KHÔNG. Mặt khác, hai hình tam giác có thể tạo thành một cấu trúc đối xứng nếu chúng được sắp xếp dưới dạng ảnh phản chiếu. Bất kỳ cách tiếp cận nào chỉ kiểm tra tính phân chia diện tích hoặc bỏ qua các yêu cầu về tính đối xứng sẽ cho rằng tất cả$n$hợp lệ vì diện tích luôn chia hết và không có ràng buộc đóng gói trong câu lệnh. 

## Phương pháp tiếp cận 

Một cách ngây thơ để suy nghĩ về vấn đề này là tưởng tượng đặt từng hình tam giác một và thử tất cả các hướng và vị trí có thể có trong khi vẫn duy trì một điều kiện đối xứng tổng thể. Mỗi hình tam giác mới có thể nằm trên trục đối xứng hoặc được ghép nối với một hình đối xứng được phản chiếu. Điều này nhanh chóng trở thành tìm kiếm theo cấp số nhân trên các vị trí, vì sau khi đặt một vài hình tam giác, số lượng cấu hình hình học sẽ tăng lên theo kiểu tổ hợp. Ngay cả khi chúng ta rời rạc hóa các vị trí có thể, không gian tìm kiếm vẫn phát triển vượt xa các giới hạn khả thi. 

Quan sát quan trọng là tính đối xứng đặt ra ràng buộc ghép nối toàn cầu. Mọi tam giác không nằm chính xác trên trục đối xứng đều phải có một đối tác phản chiếu. Vì tất cả các hình tam giác đều bằng nhau và không có tính đối xứng bên trong nên không có hình tam giác nào có thể đứng một mình trên trục mà không phá vỡ tính nhất quán về hình dạng. Điều này có nghĩa là mọi hình tam giác đều phải là một phần của một cặp đối xứng. 

Điều đó ngay lập tức làm giảm vấn đề về điều kiện chẵn lẻ. Nếu như$n$chẵn, chúng ta có thể ghép các hình tam giác tùy ý và đặt mỗi cặp thành hai bản sao đối xứng tạo thành một đơn vị đối xứng. Một cấu trúc cụ thể tồn tại: hai hình tam giác 3-4-5 luôn có thể được sắp xếp để tạo thành một hình chữ nhật 3 x 4, đối xứng với cả hai đường giữa của nó. Việc lặp lại các khối được ghép nối như vậy sẽ tạo ra một hình dạng đối xứng toàn cục. 

Nếu như$n$là số lẻ, một hình tam giác sẽ luôn không ghép đôi. Tam giác còn lại đó không thể đặt trên trục đối xứng mà không phá vỡ sự tương đương gương, vì bản thân nó không có tính đối xứng phản xạ. Do đó bất kỳ điều kỳ lạ nào$n$là không thể. 

Việc khám phá vũ phu trở nên không cần thiết một khi chúng ta nhận ra rằng toàn bộ sự phức tạp về hình học tập trung vào việc liệu chúng ta có thể ghép các hình tam giác một cách hoàn hảo hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vị trí hình học Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Quan sát tính chẵn lẻ (ghép đối số đối xứng) |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$. Đây là số lượng hình tam giác vuông giống hệt nhau có sẵn để xây dựng. 
2. Kiểm tra xem$n$chia hết cho 2. Điều kiện này kiểm tra xem liệu chúng ta có thể phân chia tất cả các hình tam giác thành các cặp đối xứng hay không, điều này được yêu cầu bởi sự đối xứng trục. 
3. Nếu$n \% 2 = 0$, xuất ra CÓ. Điều này tương ứng với việc ghép mỗi tam giác với một tam giác khác và sắp xếp từng cặp đối xứng trong mặt phẳng. 
4. Nếu không thì xuất ra NO, vì ít nhất một tam giác sẽ vẫn không có đối tác đối xứng, khiến cho tính đối xứng tổng thể không thể thực hiện được. 

### Tại sao nó hoạt động 

Điều bất biến là bất kỳ sự sắp xếp đối xứng trục hợp lệ nào đều phân chia tất cả các ô thành các quỹ đạo dưới sự phản xạ: mỗi quỹ đạo có kích thước 2 (một cặp tam giác đối xứng) hoặc kích thước 1 (một tam giác nằm chính xác trên trục). Bởi vì tam giác 3-4-5 không có tính đối xứng phản xạ nên nó không thể tạo thành quỹ đạo có kích thước 1 hợp lệ để bảo toàn hình dạng khi phản xạ. Do đó mọi quỹ đạo đều phải có kích thước 2, buộc tổng số hình tam giác phải là số chẵn. Bất biến này mô tả đầy đủ tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

if n % 2 == 0:
    print("YES")
else:
    print("NO")
```Việc triển khai áp dụng trực tiếp điều kiện chẵn lẻ xuất phát ở trên. Không cần mô phỏng hình học. Điều tinh tế duy nhất là đảm bảo phân tích cú pháp đầu vào chính xác cho một số nguyên duy nhất và thực hiện kiểm tra mô đun theo thời gian không đổi. 

Quyết định không phụ thuộc rõ ràng vào hình học tam giác trong mã vì độ phức tạp đó đã được giảm xuống thành ràng buộc ghép nối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```| Bước | n | n % 2 | Quyết định | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 1 | - | - | 
| Kiểm tra tính chẵn lẻ | 1 | 1 | KHÔNG | 

Điều này chứng tỏ không thể đặt một tam giác bất đối xứng vào bất kỳ cấu hình đối xứng phản xạ nào. 

### Ví dụ 2 

đầu vào:```
4
```| Bước | n | n % 2 | Giải thích ghép nối | Quyết định | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 4 | 0 | 2 cặp tráng gương | CÓ | 

Điều này cho thấy số lượng chẵn cho phép ghép nối đầy đủ như thế nào. Mỗi cặp có thể được sắp xếp dưới dạng một hình chữ nhật đối xứng và việc kết hợp các khối như vậy sẽ bảo toàn tính đối xứng tổng thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ kiểm tra tính chẵn lẻ một lần trên số nguyên đầu vào | 
| Không gian |$O(1)$| Không có bộ nhớ bổ sung ngoài giá trị đầu vào | 

Giải pháp là thời gian không đổi và phù hợp một cách tầm thường trong các ràng buộc của$n \le 10^9$, vì không có vòng lặp hoặc cách xây dựng nào phụ thuộc vào$n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n = int(input().strip())
    return "YES\n" if n % 2 == 0 else "NO\n"

# sample-style checks
assert run("1") == "NO\n"
assert run("2") == "YES\n"

# custom cases
assert run("3") == "NO\n"
assert run("4") == "YES\n"
assert run("1000000000") == "YES\n"
assert run("999999999") == "NO\n"
assert run("10") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | KHÔNG | trường hợp tối thiểu, tam giác đơn lẻ | 
| 2 | CÓ | ghép đôi đối xứng hợp lệ nhỏ nhất | 
| 3 | KHÔNG | trường hợp lẻ vượt quá mức tối thiểu | 
| 4 | CÓ | xây dựng cặp đôi cơ bản | 
| 10 | CÓ | hành vi chẵn chung | 
| 999999999 | KHÔNG | trường hợp biên lẻ lớn | 
| 1000000000 | CÓ | trường hợp biên chẵn lớn | 

## Vỏ cạnh 

cho$n = 1$, thuật toán ngay lập tức trả về NO vì việc kiểm tra tính chẵn lẻ không thành công. Điều này phù hợp với thực tế hình học rằng một tam giác 3-4-5 không thể phản chiếu để tạo thành một hình đối xứng trục nhất quán. 

Đối với các giá trị lẻ lớn như$n = 10^9 - 1$, việc tính toán vẫn giảm xuống còn một phép toán mô đun duy nhất. Giá trị này là số lẻ nên đầu ra là KHÔNG, phản ánh rằng một tam giác vẫn chưa được ghép đôi bất kể tỷ lệ. 

Đối với các giá trị chẵn lớn như$n = 10^9$, thuật toán xuất ra CÓ. Về mặt khái niệm, tất cả các hình tam giác có thể được chia thành$n/2$các cặp được nhân đôi, mỗi cặp tạo thành một đơn vị đối xứng có thể được xếp lớp mà không ảnh hưởng đến tính đối xứng tổng thể.
