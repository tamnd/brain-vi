---
title: "CF 104666I - Ponk Warshall"
description: "Chúng ta có hai chuỗi có độ dài bằng nhau trên bảng chữ cái {A, C, G, T}. Chuỗi thứ hai là hoán vị của chuỗi thứ nhất, nghĩa là cả hai đều chứa chính xác nhiều bộ ký tự giống nhau."
date: "2026-06-29T09:55:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104666
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ICPC Central Europe Regional Contest (CERC 19)"
rating: 0
weight: 104666
solve_time_s: 73
verified: false
draft: false
---

[CF 104666I - Ponk Warshall](https://codeforces.com/problemset/problem/104666/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài bằng nhau trên bảng chữ cái {A, C, G, T}. Chuỗi thứ hai là hoán vị của chuỗi thứ nhất, nghĩa là cả hai đều chứa chính xác nhiều bộ ký tự giống nhau. Nhiệm vụ là chuyển đổi chuỗi đầu tiên thành chuỗi thứ hai bằng cách sử dụng các hoán đổi, trong đó mỗi hoán đổi trao đổi hai vị trí tùy ý trong chuỗi. 

Mục tiêu là tính toán số lần hoán đổi tối thiểu cần thiết. 

Một cách hữu ích để điều chỉnh lại vấn đề là nghĩ đến việc căn chỉnh các vị trí thay vì các ký tự. Mỗi vị trí trong chuỗi đầu tiên cuối cùng phải “gửi” ký tự của nó đến một vị trí nào đó trong chuỗi thứ hai mong đợi. Điều này tạo ra một hoán vị của các chỉ số và câu hỏi trở thành: số lần hoán đổi tối thiểu cần thiết để nhận ra hoán vị này bắt đầu từ sự sắp xếp danh tính là bao nhiêu. 

Các ràng buộc cho phép độ dài chuỗi lên tới 10^6, điều này ngay lập tức loại trừ bất kỳ phương trình bậc hai hoặc thậm chí nào$O(n \log n)$cách tiếp cận dựa vào việc xây dựng lại chu trình rõ ràng với việc ghi chép sổ sách dày đặc. Bất kỳ giải pháp nào cũng phải là thời gian tuyến tính và về cơ bản là một lượt với công việc không đổi trên mỗi ký tự. 

Một trường hợp khó thấy nhưng quan trọng là khi các ký tự lặp lại nhiều. Ví dụ: nếu cả hai chuỗi giống hệt nhau thì câu trả lời là 0 và bất kỳ thuật toán nào xây dựng ánh xạ không khớp đều phải xử lý chính xác các cấu trúc trống. Một trường hợp cạnh khác là khi hoán vị bao gồm các chu kỳ dài. Ví dụ: một sự dịch chuyển theo chu kỳ của toàn bộ chuỗi tạo ra một chu kỳ có độ dài n và câu trả lời phải là n−1. Bất kỳ phương pháp sửa lỗi không khớp cục bộ tham lam nào không nhận ra chu kỳ sẽ đánh giá thấp hoặc tính quá mức trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Một quan điểm đơn giản là quét chuỗi liên tục, tìm vị trí mà chuỗi hiện tại khác với chuỗi đích và hoán đổi nó với vị trí chứa ký tự cần thiết. Điều này đúng vì mỗi lần hoán đổi có thể sửa được ít nhất một ký tự bị đặt sai vị trí. Tuy nhiên, chi phí để xác định đúng vị trí của đối tác liên tục dẫn đến hành vi bậc hai. Trong trường hợp xấu nhất, mỗi vị trí trong số n vị trí có thể yêu cầu quét một phân đoạn O(n) khác, dẫn đến các hoạt động O(n²), điều này không khả thi đối với n tối đa 10^6. 

Quan sát cấu trúc quan trọng là các giao dịch hoán đổi hoạt động theo chu kỳ trong một hoán vị ẩn giữa các vị trí. Nếu chúng ta sửa ánh xạ từ mỗi vị trí trong chuỗi đầu tiên sang vị trí trong chuỗi thứ hai nơi ký tự đó sẽ xuất hiện thì chúng ta sẽ thu được hoán vị của các chỉ số. Mọi hoán đổi sẽ hợp nhất hoặc phá vỡ các chu kỳ theo cách có thể dự đoán được và số lượng hoán đổi tối thiểu để sắp xếp một hoán vị được xác định hoàn toàn bằng cách phân tách chu trình của nó. 

Mỗi chu kỳ có độ dài k yêu cầu chính xác k−1 lần hoán đổi để được cố định một cách tối ưu. Điều này làm giảm vấn đề xác định các chu kỳ trong ánh xạ vị trí và tổng hợp những đóng góp của chúng. 

Sự phức tạp duy nhất là xây dựng ánh xạ một cách hiệu quả dưới các ký tự lặp lại. Vì các chữ cái không phải là duy nhất nên chúng ta không thể ánh xạ trực tiếp theo giá trị. Thay vào đó, chúng tôi so khớp các lần xuất hiện bằng cách sử dụng hàng đợi: đối với mỗi ký tự, lưu trữ các chỉ mục nơi nó xuất hiện trong chuỗi đích, sau đó gán các lần xuất hiện từ chuỗi nguồn cho các vị trí này theo thứ tự. 

Sau khi ánh xạ được xây dựng, phần còn lại là tính chu kỳ tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trao đổi địa phương tham lam với tìm kiếm | O(n²) | O(n) | Quá chậm | 
| Phân rã chu trình thông qua khớp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hoán vị được tạo ra bằng cách căn chỉnh các ký tự từ chuỗi đầu tiên sang chuỗi thứ hai. 

1. Đối với mỗi ký tự trong chuỗi đích, hãy lưu trữ một hàng chỉ mục của nó. Điều này chuẩn bị việc so khớp xác định cho các bản sao, vì các chữ cái giống hệt nhau phải được so khớp để duy trì tính khả thi. 
2. Duyệt chuỗi nguồn từ trái sang phải. Đối với mỗi ký tự, hãy gán nó vào vị trí không được sử dụng sớm nhất trong hàng đợi tương ứng trong mục tiêu. Điều này xây dựng một mảng`to[i]`nghĩa là ký tự ở vị trí i trong nguồn phải đến vị trí`to[i]`trong mục tiêu. 
3. Bây giờ chúng tôi giải thích`to`như một hoán vị trên các chỉ số từ 0 đến n−1. Nhiệm vụ trở thành tìm số lần hoán đổi tối thiểu để thực hiện hoán vị này. 
4. Đánh dấu tất cả các vị trí chưa được xem, sau đó lặp qua các chỉ mục. Khi chúng tôi tìm thấy một chỉ mục chưa được truy cập, chúng tôi sẽ duyệt qua chu trình của nó bằng cách liên tục theo dõi`to[i]`cho đến khi chúng ta quay trở lại nút đã truy cập. 
5. Với mỗi chu kỳ có độ dài k, hãy thêm k−1 vào câu trả lời. Điều này tương ứng với thực tế là một chu trình có thể được sửa chữa bằng cách liên tục hoán đổi một phần tử vào vị trí cuối cùng của nó, mỗi lần giảm kích thước chu trình đi một phần tử. 

### Tại sao nó hoạt động 

Ánh xạ được xây dựng là một phép ánh xạ vì cả hai chuỗi đều có nhiều bộ ký tự giống hệt nhau và mỗi lần xuất hiện được sử dụng chính xác một lần. Như vậy`to`phân hủy thành các chu trình rời rạc bao trùm tất cả các chỉ số. Bất kỳ sự hoán đổi nào giữa hai vị trí đều có thể làm giảm số lượng chu kỳ hoặc hợp nhất chúng, nhưng không thể giảm tổng số lần hoán đổi cần thiết xuống dưới tổng (độ dài chu kỳ - 1). Vì mỗi chu kỳ là độc lập và chúng ta có thể sửa từng chu kỳ một cách rõ ràng trong k−1 lần hoán đổi nên kết quả là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    t = input().strip()
    n = len(s)

    pos = {'A': [], 'C': [], 'G': [], 'T': []}
    for i, ch in enumerate(t):
        pos[ch].append(i)

    ptr = {'A': 0, 'C': 0, 'G': 0, 'T': 0}
    to = [0] * n

    for i, ch in enumerate(s):
        to[i] = pos[ch][ptr[ch]]
        ptr[ch] += 1

    vis = [False] * n
    ans = 0

    for i in range(n):
        if vis[i]:
            continue
        cur = i
        size = 0
        while not vis[cur]:
```
