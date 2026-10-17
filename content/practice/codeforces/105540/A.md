---
title: "CF 105540A - Kẻ Khờ"
description: "Chúng ta có một lưới hình chữ nhật có $n$ hàng và $m$ cột. Mỗi ô không phải là một ký tự đơn mà là một chuỗi ngắn có độ dài cố định $k$. Vì vậy, mọi vị trí trong lưới đều lưu trữ một “từ” và tất cả các từ trong lưới phải tuân theo một mẫu rất nghiêm ngặt."
date: "2026-06-27T00:55:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105540
codeforces_index: "A"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Jinan Site (The 3rd Universal Cup. Stage 17: Jinan)"
rating: 0
weight: 105540
solve_time_s: 51
verified: true
draft: false
---

[CF 105540A - Kẻ ngốc](https://codeforces.com/problemset/problem/105540/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một lưới hình chữ nhật có$n$hàng và$m$cột. Mỗi ô không phải là một ký tự đơn mà là một chuỗi ngắn có độ dài cố định$k$. Vì vậy, mọi vị trí trong lưới đều lưu trữ một “từ” và tất cả các từ trong lưới phải tuân theo một mẫu rất nghiêm ngặt. 

Có chính xác một ô đặc biệt trong lưới. Tất cả khác$n \cdot m - 1$các ô chứa cùng một chuỗi, trong khi chuỗi đặc biệt khác với chuỗi chung này ở ít nhất một ký tự. Lưới được đảm bảo chứa chính xác một ngoại lệ như vậy và nhiệm vụ của chúng ta là xác định vị trí của nó bằng cách xuất ra hàng và cột của nó. 

Cấu trúc rất quan trọng: thay vì so sánh từng ký tự riêng lẻ, chúng ta so sánh toàn bộ khối có độ dài$k$. Định dạng đầu vào nối tất cả$m \cdot k$ký tự trên mỗi hàng, vì vậy chúng ta phải xây dựng lại từng ô trước khi suy luận. 

Ràng buộc$n, m \le 200$ngụ ý nhiều nhất$40{,}000$tế bào và$k \le 10$làm cho việc trích xuất mỗi mã thông báo trở nên rẻ. Điều này ngay lập tức loại trừ bất cứ điều gì phức tạp hơn việc quét tuyến tính trên tất cả các ô, vì thậm chí$O(nm \cdot k)$hoạt động là tầm thường. 

Một cách tiếp cận đơn giản để so sánh từng cặp tế bào sẽ là$O((nm)^2 \cdot k)$, trong trường hợp xấu nhất đạt tới hàng tỷ so sánh và không cần thiết. 

Một số trường hợp đặc biệt đáng được tách biệt. 

Nếu ô lẻ chỉ khác nhau ở ký tự cuối cùng trong chuỗi của nó, việc kiểm tra tần số chung theo từng ký tự vẫn phát hiện chính xác ô đó, nhưng phương pháp băm toàn chuỗi đơn giản phải đảm bảo không có xung đột hoặc lỗi định dạng. 

Nếu như$k = 1$, vấn đề sẽ suy biến thành việc tìm phần tử duy nhất trong một lưới trong đó tất cả trừ một mục đều giống hệt nhau. Bất kỳ phương pháp nào giả định chuỗi nhiều ký tự vẫn phải xử lý trường hợp này mà không có lỗi viết hoa đặc biệt. 

Nếu chuỗi bất thường trông giống với chuỗi thông thường về mặt hình ảnh nhưng khác nhau ở một ký tự duy nhất, việc quét một phần theo hàng hoặc theo cột vẫn tách biệt chính xác chuỗi đó, nhưng chỉ khi việc so sánh được thực hiện trên các mã thông báo đầy đủ thay vì tiền tố. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi xây dựng lại từng chuỗi ô và so sánh từng chuỗi với tất cả các chuỗi khác. Đối với mỗi ô ứng cử viên, chúng tôi đếm có bao nhiêu chuỗi giống hệt nhau tồn tại trong lưới. Ô có chuỗi xuất hiện đúng một lần là câu trả lời. 

Điều này hiệu quả vì vấn đề đảm bảo một ngoại lệ duy nhất. Tuy nhiên, nó trở nên quá chậm vì việc so sánh tất cả các cặp yêu cầu$O((nm)^2 \cdot k)$hoạt động. Với$nm = 40{,}000$, điều này dẫn đến khoảng$1.6 \times 10^9$so sánh, vượt quá giới hạn điển hình. 

Quan sát quan trọng là chúng ta không cần so sánh theo cặp. Vì tất cả các ô bình thường đều giống hệt nhau nên chuỗi đúng sẽ xuất hiện ít nhất hai lần. Số lẻ xuất hiện đúng một lần. Điều này biến vấn đề thành một nhiệm vụ nhận dạng tần số. 

Vì vậy, thay vì so sánh các cấu trúc, chúng tôi nén từng ô thành một mã định danh duy nhất (chuỗi đầy đủ của nó), lưu trữ số lượng và xác định vị trí xuất hiện duy nhất. Điều này làm giảm vấn đề xuống còn quét tuyến tính cộng với băm hoặc đếm từ điển. 

Cấu trúc của bố cục chuỗi đầu vào cũng có vấn đề: mỗi hàng là một chuỗi của$m$khối có chiều dài$k$, vì vậy việc phân tích cú pháp chính xác là điều cần thiết. Sau khi được phân tích cú pháp, mọi thứ sẽ giảm xuống thành vấn đề tần số tiêu chuẩn$nm$mặt hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| So sánh cặp Brute Force |$O((nm)^2 \cdot k)$|$O(1)$| Quá chậm | 
| Đếm tần số (bản đồ băm) |$O(nm \cdot k)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách coi mỗi ô là một mã thông báo chuỗi và đếm số lần xuất hiện. 

1. Đọc$n, m, k$. Chúng xác định số lượng ô tồn tại và độ lớn của mỗi chuỗi ô. 
2. Đối với mỗi hàng, hãy đọc toàn bộ chuỗi có độ dài được nối$m \cdot k$. Sau này chúng ta sẽ cắt nó thành các khối. 
3. Chia mỗi hàng thành$m$chuỗi con, mỗi chuỗi có độ dài$k$. Mỗi chuỗi con đại diện cho một ô lưới tại vị trí$(i, j)$. 
4. Chèn từng chuỗi ô vào bản đồ băm (hoặc từ điển) để theo dõi số lần nó xuất hiện. Trong khi làm như vậy, cũng lưu trữ vị trí xuất hiện đầu tiên của nó. 
5. Sau khi xử lý tất cả các ô, hãy quét bản đồ tần số và tìm chuỗi xuất hiện đúng một lần. Đây là tế bào dị thường. 
6. Xuất tọa độ được lưu trữ tương ứng với chuỗi duy nhất đó. 

Lý do chúng tôi lưu trữ tọa độ trong quá trình chèn là vì việc tính toán lại các vị trí sau này sẽ yêu cầu một lần chuyển qua lưới khác, điều này là không cần thiết. 

### Tại sao nó hoạt động 

Tất cả các ô không đặc biệt đều giống hệt nhau theo định nghĩa bài toán, vì vậy chúng đóng góp cùng một khóa trong bản đồ tần số. Ô đặc biệt khác nhau ở ít nhất một ký tự, điều này đảm bảo nó tạo thành một khóa riêng biệt. Vì có chính xác một ô như vậy nên tần số của nó chính xác là một ô, trong khi tất cả các ô khác có tần số ít nhất một lớn hơn hoặc bằng số lượng bản sao. Ràng buộc duy nhất đảm bảo không có sự mơ hồ trong lựa chọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m, k = map(int, input().split())
    
    freq = {}
    pos = {}
    
    for i in range(n):
        row = input().strip()
        for j in range(m):
            cell = row[j * k:(j + 1) * k]
            if cell not in freq:
                freq[cell] = 0
                pos[cell] = (i + 1, j + 1)
            freq[cell] += 1

    for cell, cnt in freq.items():
        if cnt == 1:
            r, c = pos[cell]
            print(r, c)
            return

if __name__ == "__main__":
    main()
```Chi tiết chính trong quá trình triển khai là cắt lát cẩn thận: mỗi ô bắt đầu tại chỉ mục$j \cdot k$và kết thúc tại$(j+1)\cdot k$. Lỗi ngẫu nhiên ở đây là nguồn lỗi phổ biến nhất. 

Chúng tôi cũng đảm bảo rằng việc lập chỉ mục hàng được chuyển đổi từ dựa trên 0 sang dựa trên một khi lưu trữ tọa độ đầu ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử$n = 2, m = 3, k = 2$và lưới giải mã thành: 

Hàng 1: AA BB BB 

Hàng 2: AA BB CC 

| Bước | Tế bào | Trạng thái bản đồ tần số | 
| --- | --- | --- | 
| 1 | AA | {AA: 1} | 
| 2 | BB | {AA: 1, BB: 1} | 
| 3 | BB | {AA: 1, BB: 2} | 
| 4 | AA | {AA: 2, BB: 2} | 
| 5 | BB | {AA: 2, BB: 3} | 
| 6 | CC | {AA: 2, BB: 3, CC: 1} | 

Phần tử duy nhất là CC, nằm ở vị trí đã ghi của nó. 

Dấu vết này cho thấy chúng ta không bao giờ cần so sánh trực tiếp các vị trí mà chỉ cần so sánh tần số. 

### Ví dụ 2 

Hãy xem xét một lưới trong đó ô lẻ chỉ khác nhau một ký tự: 

Hàng 1: XY XY XY 

Hàng 2: XY XX XY 

| Bước | Tế bào | Trạng thái bản đồ tần số | 
| --- | --- | --- | 
| 1 | XY | {XY: 1} | 
| 2 | XY | {XY: 2} | 
| 3 | XY | {XY: 3} | 
| 4 | XY | {XY: 4} | 
| 5 | Z | {XY: 4, Hz: 1} | 
| 6 | XY | {XY: 5, Hz: 1} | 

Một lần nữa, Xperia có khả năng nhận dạng duy nhất bất kể nó có gần gũi với số đông đến mức nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm \cdot k)$| mỗi cái$nm$các ô được cắt lát và băm theo chiều dài$k$| 
| Không gian |$O(nm)$| trường hợp xấu nhất tất cả các ô đều là các khóa riêng biệt trong bản đồ | 

Giới hạn$n, m \le 200$Và$k \le 10$làm cho việc này trở nên hiệu quả một cách thoải mái, vì tổng số ký tự nhiều nhất là$4 \times 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    return stdout.getvalue()

# Since full solution is not wrapped as function here, these are structural examples only.

# custom cases
# 1) minimal grid
# 2) unique at end
# 3) differing by one character
# 4) all structure edge
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 2 2 1 có ô cuối cùng khác nhau | đúng vị trí | trường hợp tối thiểu | 
| 3x3 với ô cuối cùng duy nhất | dưới cùng bên phải | lập chỉ mục ranh giới | 
| sự khác biệt một ký tự | phát hiện đúng | tính đúng đắn của chuỗi con | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi ô đặc biệt chỉ khác nhau một ký tự duy nhất bên trong một chuỗi giống hệt nhau. Trong trường hợp đó, việc nhóm dựa trên tiền tố sẽ không thành công, nhưng việc băm toàn bộ chuỗi sẽ phân tách nó một cách chính xác. 

Một trường hợp khác là khi ô duy nhất xuất hiện ở hàng đầu tiên hoặc cột đầu tiên. Vì chúng tôi lưu trữ vị trí ở lần gặp đầu tiên nên chúng tôi nắm bắt chính xác tọa độ ngay cả khi xử lý các bản sao sau đó. 

Trường hợp cuối cùng là khi tất cả các ô trông gần giống nhau nhưng khác nhau ở các ký tự ASCII không in được. Bởi vì chúng tôi coi các chuỗi là các chuỗi byte thô nên việc đếm tần số vẫn đáng tin cậy và không bị ảnh hưởng bởi ngữ nghĩa ký tự.
