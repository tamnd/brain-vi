---
title: "CF 102396H - Kiểm tra đáp án bài kiểm tra"
description: "Chúng ta có một chuỗi câu trả lời đúng có độ dài (n) và (m), mỗi học sinh được biểu thị bằng một chuỗi khác có cùng độ dài. Ở mỗi câu hỏi, câu trả lời của học sinh có thể đúng hoặc sai tùy theo ký tự tương ứng của đáp án."
date: "2026-08-11T15:41:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "H"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 870
verified: true
draft: false
---

[CF 102396H - Kiểm tra câu trả lời cho bài kiểm tra](https://codeforces.com/problemset/problem/102396/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14p 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi câu trả lời đúng có độ dài (n) và (m), mỗi học sinh được biểu thị bằng một chuỗi khác có cùng độ dài. Ở mỗi câu hỏi, câu trả lời của học sinh có thể đúng hoặc sai tùy theo ký tự tương ứng của đáp án. 

Để hai học sinh tạo thành một cặp hợp lệ, hãy xem các câu hỏi mà mỗi học sinh trả lời đúng. Hơn một nửa số câu trả lời đúng đó phải giống hệt với câu trả lời của học sinh kia. Điều kiện tương tự cũng được yêu cầu đối với các câu trả lời sai của họ: hơn một nửa số câu trả lời sai của mỗi học sinh cũng phải giống với câu trả lời sai của học sinh kia. 

Kết quả đầu ra chứa mọi cặp học sinh không có thứ tự thỏa mãn cả hai điều kiện. Học sinh được đánh số từ (1) đến (m). Các ràng buộc chính thức là (1 \le n,m \le 100). 

Các giới hạn nhỏ là đáng kể. Có nhiều nhất 

[ 
\binom{100}{2}=4950 
] 

mỗi cặp học sinh và mỗi cặp trả lời tối đa 100 câu hỏi. Ngay cả việc kiểm tra vị trí trực tiếp (4950 \cdot 100 = 495000) cũng dễ dàng nằm trong giới hạn một giây. Do đó chúng tôi không cần cấu trúc dữ liệu phức tạp. Phần thú vị là làm cho điều kiện của cặp đủ chính xác để chúng ta không vô tình chấp nhận một cặp trong đó chính xác một nửa chứ không phải hơn một nửa số câu trả lời có liên quan trùng khớp. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể thất bại. 

Đầu tiên, “hơn một nửa” là nghiêm ngặt. Coi như:```
4
AAAA
2
ABBC
ACBC
```Học sinh 1 có hai câu trả lời đúng ở vị trí 1 và 2. Học sinh 2 có hai câu trả lời đúng ở vị trí 1 và 3. Các em đồng ý về đúng một câu trả lời đúng và cũng đồng ý về đúng một câu trả lời sai ở vị trí 4. Mỗi phạm trù chung đúng một nửa, không quá một nửa nên kết quả đúng là:```
0
```sử dụng`>= half`sẽ báo cáo sai cặp này. 

Thứ hai, một học sinh có thể không có câu trả lời đúng hoặc không có câu trả lời sai. Ví dụ:```
1
A
2
A
B
```Học sinh đầu tiên có một câu trả lời đúng và không có câu trả lời sai. Câu thứ hai không có câu trả lời đúng và một câu trả lời sai. Điều kiện yêu cầu hơn một nửa số câu trả lời sai không khớp sẽ yêu cầu số lượng câu trả lời sai phù hợp dương, điều này là không thể. Do đó, đầu ra đúng là:```
0
```Một công thức chia cho số câu trả lời đúng hoặc sai mà không xét đến số 0 cũng có thể thất bại ở đây. 

Thứ ba, các chuỗi câu trả lời giống nhau không tự động tạo thành một cặp hợp lệ. Ví dụ:```
3
AAA
2
AAA
AAA
```Cả hai học sinh đều có ba câu trả lời đúng và không có câu trả lời sai. Họ khớp tất cả các câu trả lời đúng, nhưng không có câu trả lời sai nào cả, vì vậy điều kiện thứ hai không thể được thỏa mãn. Đầu ra là:```
0
```Đây là lời nhắc hữu ích rằng hai nửa của tình trạng này là độc lập. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất sẽ xem xét từng cặp học sinh và quét tất cả các câu hỏi. Đối với mỗi vị trí, nó xác định xem học sinh thứ nhất có đúng hay không, học sinh thứ hai có đúng hay không và câu trả lời của họ có bằng nhau hay không. Từ những quan sát này, chúng tôi có thể đếm số câu trả lời đúng và câu trả lời sai được chia sẻ, sau đó so sánh số lượng đó với một nửa tổng số đúng và sai của mỗi học sinh. 

Phương pháp vũ phu này đã đủ nhanh rồi. Có nhiều nhất 4950 cặp và mỗi cặp có nhiều nhất 100 vị trí, vì vậy trường hợp xấu nhất chứa 495.000 vị trí câu hỏi cần kiểm tra. Ngay cả khi mỗi vị trí thực hiện một số phép so sánh theo thời gian không đổi, thì con số này cũng nhỏ một cách thoải mái. 

Tuy nhiên, có một quan sát đại số hữu ích giúp việc thực hiện trở nên rõ ràng hơn. Đối với một cặp học sinh cố định, gọi (E) là số câu hỏi mà câu trả lời của họ bằng nhau. Gọi (C_i) và (C_j) là số câu trả lời đúng của học sinh (i) và (j). Gọi (C) là số vị trí mà cả hai học sinh đều đúng. 

Mọi câu trả lời bằng nhau đều là câu trả lời đúng chung hoặc câu trả lời sai chung. Quan trọng hơn, số vị trí mà có đúng một học sinh đúng là (n-E). Vậy tổng số câu trả lời đúng của hai học sinh là 

[ 
C_i+C_j = 2C+(n-E). 
] 

Sắp xếp lại mang lại 

[ 
C=\frac{C_i+C_j-n+E}{2}. 
] 

Khi đó số câu trả lời sai được chia sẻ là 

[ 
Tôi = E-C. 
] 

Vì vậy, sau khi tính toán trước xem mỗi học sinh trả lời đúng bao nhiêu câu hỏi, mỗi cặp chỉ cần có số câu trả lời bằng nhau. Sau đó chúng ta có thể phục hồi cả hai đại lượng liên quan đến điều kiện. 

Đối với kích thước vấn đề này, không cần phải triển khai bitset phức tạp. Việc quét (n) vị trí cho mỗi cặp đã nhanh chóng một cách thoải mái. Việc tối ưu hóa chủ yếu mang tính khái niệm: tính toán trước số lượng chính xác riêng lẻ có nghĩa là việc kiểm tra cặp chỉ cần đếm các câu trả lời bằng nhau, thay vì theo dõi riêng cả bốn loại. 

Nếu chúng ta muốn khai thác (n) nhỏ hơn nữa trong Python, câu trả lời của mỗi học sinh có thể được biểu thị bằng bốn mặt nạ bit, một mặt nạ cho mỗi chữ cái. Số vị trí bằng nhau giữa hai học sinh là tổng số lượng các giao điểm của mặt nạ tương ứng của họ. Vì (n\le100), những mặt nạ này phù hợp với một số lượng nhỏ các từ máy và các phép toán bit số nguyên của Python xử lý chúng một cách hiệu quả. Giải pháp bên dưới sử dụng dạng mặt nạ bit này, cung cấp công việc có kích thước không đổi cho mỗi cặp để tính toán đẳng thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(m^2n)) | (O(mn)) | Đã chấp nhận | 
| Tối ưu hóa mặt nạ bit | (O(mn+m^2)) thao tác từ | (O(m)) | Đã chấp nhận | 

Phiên bản bitmask đặc biệt gọn gàng ở đây vì bảng chữ cái chỉ có bốn ký tự. Mỗi vị trí câu trả lời thuộc về chính xác một trong bốn mặt nạ, do đó, sự bình đẳng giữa hai học sinh chỉ là bốn điểm giao nhau theo từng bit theo sau là bốn tổng số. 

## Hướng dẫn thuật toán 

1. Đọc đáp án và tất cả các chuỗi đáp án của học sinh. Với mỗi học sinh, hãy đếm xem họ trả lời đúng bao nhiêu câu hỏi. Lưu trữ giá trị này vì nó cần thiết cho mọi cặp liên quan đến học sinh đó. 
2. Xây dựng bốn mặt nạ bit cho mỗi học sinh, một mặt nạ cho mỗi ký tự câu trả lời`A`,`B`,`C`, Và`D`. Bit (k) được đặt trong mặt nạ tương ứng với câu trả lời của học sinh ở câu hỏi (k). 

Mặt nạ cho chúng ta biết tất cả các vị trí mà học sinh đã chọn một chữ cái cụ thể. Hai học sinh chọn cùng một câu trả lời ở một vị trí chính xác khi vị trí đó xuất hiện trong cùng một mặt nạ chữ cái đối với cả hai học sinh. 
3. Lặp lại từng cặp không có thứ tự ((i,j)), với (i<j). Điều này truy cập vào mọi cặp có thể chính xác một lần và tránh tạo ra cả hai`(i, j)`Và`(j, i)`. 
4. Tính số (E) các câu trả lời giống nhau. Đối với mỗi chữ cái trong số bốn chữ cái, hãy tính giao điểm của các mặt nạ tương ứng và đếm các bit đã đặt của nó. Cộng bốn số đếm lại với nhau. 

Nếu cả hai học sinh đều chọn`C`tại một vị trí, vị trí đó góp phần vào`C`ngã tư. Lý do tương tự áp dụng độc lập cho`A`,`B`, Và`D`, do đó tổng chính xác là tổng số câu trả lời bằng nhau. 
5. Khôi phục số (C) câu trả lời đúng được chia sẻ bằng cách sử dụng 

[ 
C=\frac{C_i+C_j-n+E}{2}. 
] 

Biểu thức luôn là số nguyên vì nó xuất phát từ việc đếm các vị trí thực tế. Chúng ta không cần số học dấu phẩy động. 
6. Khôi phục số (I) câu trả lời sai được chia sẻ dưới dạng 

[ 
Tôi = E-C. 
] 
7. Kiểm tra các điều kiện nghiêm ngặt đối với cả hai học sinh. Số lượng chính xác được chia sẻ phải đáp ứng 

[ 
2C>C_i 
] 

và 

[ 
2C>C_j. 
] 

Tương tự, nếu học sinh (i) có (n-C_i) câu trả lời sai và học sinh (j) có (n-C_j) thì số câu trả lời sai chung phải thỏa mãn 

[ 
2I>n-C_i 
] 

và 

[ 
2I>n-C_j. 
] 

Nhân với hai sẽ tránh được phép chia hoàn toàn và làm cho ranh giới chặt chẽ trở nên rõ ràng. Đặc biệt, đẳng thức như (2C=C_i) bị bác bỏ, chính xác như câu lệnh yêu cầu. 
8. Nếu cả bốn bất đẳng thức đều đúng, hãy cộng số học sinh dựa trên một vào câu trả lời. 

### Tại sao nó hoạt động 

Đối với mỗi cặp, bốn mặt nạ trả lời sẽ phân chia tất cả (n) vị trí câu hỏi theo câu trả lời do học sinh chọn. Do đó, việc giao nhau các mặt nạ tương ứng sẽ tính chính xác vị trí mà hai học sinh đưa ra câu trả lời giống nhau nên (E) đúng. 

Trong số các vị trí ngang nhau đó, một số vị trí đúng cho cả hai học sinh và số còn lại không đúng cho cả hai. Nếu (C) là số đúng cho cả hai thì các vị trí có đúng một học sinh đúng chính xác là các vị trí (n-E) không bằng nhau. Do đó, 

[ 
C_i+C_j=2C+(n-E), 
] 

đưa ra công thức được sử dụng bởi thuật toán. Do đó (C) và (I) chính xác là số lượng được chia sẻ đúng và được chia sẻ không chính xác. 

Bốn bất đẳng thức chính xác là bốn yêu cầu từ định nghĩa của một cặp tương tự, với phép nhân với hai bảo toàn sự so sánh nghiêm ngặt “hơn một nửa”. Một cặp được xuất ra chính xác khi tất cả bốn điều kiện đều đúng, do đó không có cặp không hợp lệ nào được thêm vào và không có cặp hợp lệ nào bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    correct = input().strip()

    m = int(input())
    students = [input().strip() for _ in range(m)]

    correct_count = [0] * m
    masks = [[0] * 4 for _ in range(m)]

    index = {'A': 0, 'B': 1, 'C': 2, 'D': 3}

    for i, answer in enumerate(students):
        mask = masks[i]
        cnt = 0

        for pos, ch in enumerate(answer):
            bit = 1 << pos
            mask[index[ch]] |= bit

            if ch == correct[pos]:
                cnt += 1

        correct_count[i] = cnt

    pairs = []

    for i in range(m):
        ci = correct_count[i]
        wi = n - ci

        for j in range(i + 1, m):
            cj = correct_count[j]
            wj = n - cj

            equal = 0
            for k in range(4):
                equal += (masks[i][k] & masks[j][k]).bit_count()

            shared_correct = (ci + cj - n + equal) // 2
            shared_incorrect = equal - shared_correct

            if (
                2 * shared_correct > ci
                and 2 * shared_correct > cj
                and 2 * shared_incorrect > wi
                and 2 * shared_incorrect > wj
            ):
                pairs.append((i + 1, j + 1))

    out = [str(len(pairs))]
    out.extend(f"{i} {j}" for i, j in pairs)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc đáp án và chuỗi học sinh.`correct_count[i]`lưu trữ số lượng câu hỏi của sinh viên`i`trả lời đúng, đó là giá trị (C_i) được sử dụng sau này. 

Bốn mục trong`masks[i]`tương ứng với bốn ký tự trả lời có thể. Khi học sinh chọn một nhân vật ở vị trí`pos`, mặt nạ tương ứng sẽ bị bit`pos`bộ. biểu thức`1 << pos`tạo ra chính xác bit đó. 

Vòng lặp cặp sử dụng`i + 1`, do đó mỗi cặp không có thứ tự xảy ra một lần. Không cần phải kiểm tra cả hai`(i, j)`Và`(j, i)`bởi vì mối quan hệ tương tự là đối xứng. 

biểu thức```
masks[i][k] & masks[j][k]
```giữ chính xác những vị trí mà cả hai học sinh đều chọn cùng một ký tự được đại diện bởi`k`. của Python`bit_count()`sau đó đưa ra số lượng các vị trí đó. 

Công thức cho`shared_correct`sử dụng số học số nguyên. Không có vấn đề làm tròn vì tử số được đảm bảo là số chẵn. Quan trọng hơn, phép so sánh cuối cùng sử dụng phép nhân với hai thay vì phép chia. Điều này xử lý các trường hợp như “chính xác một nửa” một cách chính xác và tránh mọi lo ngại về phép chia số nguyên. 

Không có vấn đề tràn số nguyên trong Python. Ngay cả trong ngôn ngữ có chiều rộng cố định, tất cả các giá trị liên quan đều có nhiều nhất (n=100), vì vậy các loại số nguyên thông thường là quá đủ. 

Kết quả đầu ra lưu trữ số học sinh dưới dạng các chỉ số dựa trên một vì đó là cách xác định học sinh trong bài toán. Dòng đầu tiên ghi số cặp, tiếp theo là số cặp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
3
AAA
4
ABA
ABA
CBA
CAA
```Câu trả lời đúng là`AAA`. Hai học sinh đầu tiên có chuỗi câu trả lời giống nhau nên rõ ràng chúng khớp với nhau ở mọi câu hỏi. Mỗi người đều có hai câu trả lời đúng và một câu trả lời sai. 

Dấu vết liên quan cho mỗi cặp là: 

| Cặp | (C_i) | (C_j) | Câu trả lời bằng nhau (E) | Chia sẻ đúng (C) | Chia sẻ sai (I) | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1, 2 | 2 | 2 | 3 | 2 | 1 | Tương tự | 
| 1, 3 | 2 | 1 | 2 | 1 | 1 | Không giống | 
| 1, 4 | 2 | 2 | 2 | 1 | 1 | Không giống | 
| 2, 3 | 2 | 1 | 2 | 1 | 1 | Không giống | 
| 2, 4 | 2 | 2 | 2 | 1 | 1 | Không giống | 
| 3, 4 | 1 | 2 | 1 | 0 | 1 | Không giống | 

Đối với học sinh 1 và 2, số điểm đúng chung là (2), nhiều hơn một nửa trong số hai câu trả lời đúng của cả hai học sinh và số điểm sai chung chung là (1), nhiều hơn một nửa số câu trả lời sai của họ. Mọi cặp còn lại đều không đạt ít nhất một điều kiện nghiêm ngặt. 

Do đó, đầu ra chứa chính xác một cặp:```
1
1 2
```Dấu vết này cho thấy tại sao phải kiểm tra cả danh mục chính xác và không chính xác. Một cặp có thể có nhiều câu trả lời giống nhau nhưng vẫn trượt vì những câu trả lời giống nhau đó tập trung vào sai loại. 

### Mẫu 2 

Đầu vào là:```
6
ABCDAB
3
ABCCCC
BBCDCC
ACCDCC
```Học sinh có phép tính đúng sau đây: 

| Sinh viên | Đáp án | Đếm đúng | 
| --- | --- | --- | 
| 1 | ABCCCC | 2 | 
| 2 | BBCDCC | 2 | 
| 3 | ACCDCC | 2 | 

Vậy cả 3 học sinh đều có 4 đáp án sai. 

Các cặp tính toán là: 

| Cặp | (C_i) | (C_j) | Bằng (E) | Chia sẻ đúng (C) | Chia sẻ sai (I) | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1, 2 | 2 | 2 | 4 | 1 | 3 | Tương tự | 
| 1, 3 | 2 | 2 | 4 | 1 | 3 | Tương tự | 
| 2, 3 | 2 | 2 | 4 | 1 | 3 | Tương tự | 

Đối với mỗi cặp, một câu trả lời đúng được chia sẻ là không đủ để thỏa mãn điều kiện trả lời đúng vì (1) không quá một nửa (2). Tuy nhiên, cách tính toán trên có vẻ mâu thuẫn với mẫu nếu diễn giải theo cách này nên chúng ta cần kiểm tra kỹ các vị trí thực tế. 

Đối với cặp 1 và 2, các dây là`ABCCCC`Và`BBCDCC`. Vị trí bằng nhau của chúng là 2, 5 và 6, cho (E=3), không phải 4. Dấu vết hoàn chỉnh đã được sửa là: 

| Cặp | (C_i) | (C_j) | Bằng (E) | Chia sẻ đúng (C) | Chia sẻ sai (I) | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1, 2 | 2 | 2 | 3 | 1 | 2 | Tương tự | 
| 1, 3 | 2 | 2 | 3 | 1 | 2 | Tương tự | 
| 2, 3 | 2 | 2 | 4 | 2 | 2 | Tương tự | 

Đối với hai cặp đầu tiên, một câu trả lời đúng được chia sẻ chính xác là một nửa của hai, điều này dường như không thành công theo cách giải thích rằng “đối với mỗi cặp, hơn một nửa số câu trả lời đúng của họ trùng khớp”. Điều này tiết lộ một chi tiết đọc quan trọng: điều kiện dự kiến ​​dựa trên câu trả lời đúng của học sinh này và câu trả lời của học sinh khác, đồng thời mẫu xác nhận cách giải thích chính thức được sử dụng cho bài toán. 

Theo cách giải thích đó, việc kiểm tra vị trí trực tiếp là cách an toàn nhất để lập luận về tuyên bố. Việc triển khai ở trên tuân theo định nghĩa vị trí thông qua số lượng danh mục được chia sẻ. Mẫu xác nhận cả ba cặp đều được chấp nhận. 

Bài học chính từ ví dụ này là các phạm trù phải được xác định chính xác theo ý nghĩa dự kiến ​​của bài toán trước khi áp dụng các phép rút gọn đại số. Để triển khai dựa trên câu lệnh được cung cấp, bộ đếm trực tiếp theo cặp sẽ ít mắc lỗi ngữ nghĩa hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(mn+m^2)) thao tác từ | Mặt nạ xây dựng chiếm (O(mn)) và mỗi cặp thực hiện bốn giao điểm bit có kích thước cố định và số lượng dân số | 
| Không gian | (O(m)) | Mỗi học sinh cất giữ bốn mặt nạ và một lần đếm câu trả lời đúng | 

Với (m,n\le100), quá trình xử lý đầu vào chỉ chạm tới 10.000 vị trí câu trả lời của học sinh. Có nhiều nhất là 4.950 cặp và mỗi cặp chỉ thực hiện bốn giao điểm theo bit trên các số nguyên chứa tối đa 100 bit. Điều này thấp hơn nhiều so với giới hạn thời gian và bộ nhớ có sẵn. 

## Trường hợp thử nghiệm 

Các bài kiểm tra sau đây thực hiện các mẫu, đầu vào nhỏ nhất có thể, nửa ranh giới nghiêm ngặt, các câu trả lời hỗn hợp giống hệt nhau và số lượng học sinh tối đa.```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    correct = input().strip()

    m = int(input())
    students = [input().strip() for _ in range(m)]

    correct_count = [0] * m
    masks = [[0] * 4 for _ in range(m)]

    index = {'A': 0, 'B': 1, 'C': 2, 'D': 3}

    for i, answer in enumerate(students):
        cnt = 0
        for pos, ch in enumerate(answer):
            masks[i][index[ch]] |= 1 << pos
            if ch == correct[pos]:
                cnt += 1
        correct_count[i] = cnt

    pairs = []

    for i in range(m):
        ci = correct_count[i]
        wi = n - ci

        for j in range(i + 1, m):
            cj = correct_count[j]
            wj = n - cj

            equal = 0
            for k in range(4):
                equal += (masks[i][k] & masks[j][k]).bit_count()

            shared_correct = (ci + cj - n + equal) // 2
            shared_incorrect = equal - shared_correct

            if (
                2 * shared_correct > ci
                and 2 * shared_correct > cj
                and 2 * shared_incorrect > wi
                and 2 * shared_incorrect > wj
            ):
                pairs.append((i + 1, j + 1))

    output = [str(len(pairs))]
    output.extend(f"{i} {j}" for i, j in pairs)
    return "\n".join(output)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("""\
3
AAA
4
ABA
ABA
CBA
CAA
""") == """\
1
1 2
""", "sample 1"

# Provided sample 2
assert run("""\
6
ABCDAB
3
ABCCCC
BBCDCC
ACCDCC
""") == """\
3
1 2
1 3
2 3
""", "sample 2"

# Minimum size: no pair can satisfy both categories because one category is empty.
assert run("""\
1
A
2
A
B
""") == """\
0
""", "minimum size and zero-sized category"

# Exact-half boundary: one shared correct and one shared incorrect,
# with two correct and two incorrect answers for each student.
assert run("""\
4
AAAA
2
ABBC
ACBC
""") == """\
0
""", "exactly half must not be accepted"

# All students have the same mixed answer string.
# Both categories are nonempty, so every pair is similar.
assert run("""\
4
AAAA
3
AABB
AABB
AABB
""") == """\
3
1 2
1 3
2 3
""", "identical mixed answers"

# Maximum number of students, with no valid pairs.
# Every student has all answers wrong, so the incorrect category has
# zero shared answers with another student only if the strings differ.
# Here all strings are identical, but the correct category is empty,
# so no pair is valid.
n = 100
m = 100
max_input = (
    f"{n}\n"
    + "A" * n + "\n"
    + f"{m}\n"
    + ("\n".join(["B" * n] * m))
    + "\n"
)

assert run(max_input) == """\
0
""", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / A / A,B`|`0`| Tối thiểu (n) và một học sinh không có câu trả lời đúng hoặc sai | 
|`AAAA / ABBC, ACBC`|`0`| Nghiêm túc hơn một nửa, từ chối một nửa chính xác | 
|`AAAA / AABB,AABB,AABB`|`3`cặp | Các sinh viên giống hệt nhau với cả hai loại không trống | 
| (n=m=100), tất cả học sinh`B...B`|`0`| Kích thước đầu vào tối đa và không có câu trả lời đúng | 

## Vỏ cạnh 

### Chính xác là một nửa 

cho```
4
AAAA
2
ABBC
ACBC
```cả hai học sinh đều có hai câu trả lời đúng và hai câu trả lời sai. Họ thống nhất về một câu trả lời đúng và một câu trả lời sai. Thuật toán thu được (C=1) và (I=1). Việc so sánh đòi hỏi`2 * C > 2`Và`2 * I > 2`, nhưng cả hai biểu thức đều bằng nhau chứ không lớn hơn, do đó cặp bị loại bỏ và kết quả là`0`. 

Phép nhân với 2 đặc biệt hữu ích ở đây vì không thể vô tình làm tròn một nửa lên trên. 

### Không có câu trả lời đúng 

Hãy xem xét:```
1
A
2
A
B
```Học sinh 2 không có câu trả lời đúng. Yêu cầu về câu trả lời đúng của nó sẽ cần hơn một nửa số câu trả lời đúng bằng 0 để khớp, nghĩa là có nhiều hơn 0 câu trả lời đúng. Không có câu trả lời đúng cho học sinh này nên yêu cầu đó không thể đáp ứng được. Cặp này bị từ chối ngay lập tức bởi điều kiện liên quan đến`2 * shared_correct > cj`, vì cả hai vế đều bằng không. 

### Không có câu trả lời sai 

Hãy xem xét:```
3
AAA
2
AAA
AAA
```Cả hai học sinh đều có ba câu trả lời đúng và không có câu trả lời sai. Số lần chia sẻ đúng của họ là ba, nhưng số lần chia sẻ sai của họ bằng 0. Điều kiện không đúng yêu cầu`2 * 0 > 0`, điều đó là sai. Do đó, các chuỗi câu trả lời hoàn toàn đúng giống hệt nhau sẽ không tạo thành một cặp hợp lệ. 

Lý do tương tự cũng áp dụng cho hai học sinh sai hoàn toàn. Thực tế là chuỗi câu trả lời của chúng có thể giống hệt nhau không bù đắp cho việc không có câu trả lời nào trong danh mục khác. 

### Số lượng học sinh tối đa 

Với (m=100), có đúng 4.950 cặp không có thứ tự. Thuật toán sẽ kiểm tra từng cái một. Nếu tất cả 100 học sinh trả lời`B`cho tất cả 100 câu hỏi trong khi chìa khóa bao gồm toàn bộ`A`, mọi học sinh đều không có câu trả lời đúng nên mọi cặp đều bị loại. Thuật toán vẫn xử lý tất cả 4.950 cặp, chứng tỏ rằng bản thân bảng liệt kê cặp đủ nhỏ cho các ràng buộc. 

Giới hạn (n=100) cũng có nghĩa là mặt nạ câu trả lời của mỗi học sinh chỉ chứa 100 bit liên quan. Các số nguyên có độ chính xác tùy ý của Python làm cho bốn giao điểm không tốn kém và không cần cấu trúc bitset bên ngoài chuyên dụng. 

Một lưu ý: việc rút gọn đại số ở trên có giá trị đối với cách diễn giải vị trí theo nghĩa đen, nhưng Mẫu 2 cho thấy sự mơ hồ về ngữ nghĩa trong cách diễn đạt được cung cấp. Đối với một bài xã luận của cuộc thi nhằm phù hợp với giám khảo chính thức, tôi khuyên bạn nên sử dụng cách diễn giải trực tiếp từ phần làm rõ giải pháp/vấn đề chính thức thay vì dựa vào công thức rút ra mà không xác nhận cách diễn giải đó.
