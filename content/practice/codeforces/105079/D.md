---
title: "CF 105079D - Bánh Cupcake Cay"
description: "Chúng ta được cung cấp một chuỗi các loại bánh cupcake, mỗi loại có một giá trị độ cay cố định. Giám khảo ăn tất cả các bánh nướng theo thứ tự nào đó, mỗi loại chính xác một chiếc."
date: "2026-06-27T22:48:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105079
codeforces_index: "D"
codeforces_contest_name: "UTPC x WiCS Contest 04-05-23 (UT Internal)"
rating: 0
weight: 105079
solve_time_s: 68
verified: false
draft: false
---

[CF 105079D - Bánh nướng nhỏ cay](https://codeforces.com/problemset/problem/105079/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các loại bánh cupcake, mỗi loại có một giá trị độ cay cố định. Giám khảo ăn tất cả các bánh nướng theo thứ tự nào đó, mỗi loại chính xác một chiếc. Sự đóng góp của một chiếc bánh cupcake không chỉ dựa vào độ cay của riêng nó mà còn phụ thuộc vào việc bạn đã ăn bao nhiêu chiếc bánh cupcake trước đó. 

Nếu một chiếc bánh cupcake có vị cay`s`được ăn ở vị trí`j + 1`theo thứ tự, sự đóng góp của nó là`(j + 1) * (A * s + B)`. Tổng số điểm là tổng của những đóng góp này trên tất cả các bánh nướng nhỏ. Nhiệm vụ là hoán vị những chiếc bánh nướng nhỏ để tối đa hóa tổng số này. 

Khó khăn chính là hệ số nhân vị trí`(j + 1)`tăng trưởng tuyến tính nên việc đặt bánh sớm hay muộn sẽ thay đổi trọng lượng của mỗi chiếc bánh cupcake. Quyết định là sắp xếp các mục có trọng số phụ thuộc vào cả vị trí và giá trị được chuyển đổi`A * s + B`. 

Các ràng buộc cho phép lên đến`N = 100000`, vì vậy bất kỳ giải pháp nào thử tất cả các hoán vị đều không thể thực hiện được. Số lượng hoán vị giai thừa tăng vượt xa giới hạn tính toán. Ngay cả một bậc hai`O(N^2)`cách tiếp cận có nguy cơ hết thời gian tùy thuộc vào hằng số, vì vậy chúng tôi đang tìm kiếm một`O(N log N)`hoặc`O(N)`cấu trúc tham lam. 

Một điểm tinh tế là`A`Và`B`có thể tiêu cực. Điều này có nghĩa là sự đóng góp được chuyển đổi`(A * s + B)`có thể dương, âm hoặc bằng không. Điều đó làm thay đổi hành vi đặt hàng một cách đáng kể so với các vấn đề lập kế hoạch “sắp xếp theo giá trị” cổ điển. 

Một trường hợp có thể phá vỡ trực giác ngây thơ là khi tất cả các giá trị được chuyển đổi đều giống hệt nhau. Ví dụ, nếu`A = 0`, thì mỗi chiếc bánh cupcake đều góp phần`(j + 1) * B`, và việc đặt hàng không thành vấn đề. Một trường hợp phức tạp khác là khi`A < 0`, điều này đảo ngược ý nghĩa của “cay nhiều là tốt”. 

Một trường hợp lỗi minh họa nhỏ phát sinh nếu người ta giả sử sắp xếp theo`s`luôn luôn hoạt động. Nếu như`A = -1`Và`B = 0`, lớn hơn`s`các giá trị trở nên âm hơn và việc đặt chúng sớm sẽ giảm bớt trọng số của chúng, do đó thứ tự tối ưu sẽ bị đảo ngược so với trường hợp dương. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ thử mọi hoán vị của bánh nướng nhỏ, tính điểm cho mỗi lần đặt hàng và lấy giá trị tối đa. Điều này đúng vì nó đánh giá trực tiếp hàm mục tiêu, nhưng nó đòi hỏi phải đánh giá`N!`hoán vị. Ngay cả đối với`N = 10`, điều này đã trở thành ranh giới và đối với`N = 100000`nó hoàn toàn không thể thực hiện được. 

Để tìm cấu trúc, hãy viết lại phần đóng góp của một trật tự cố định`p`: 

Chúng tôi tổng hợp các vị trí`i`:`i * (A * s_{p[i]} + B)`Phân phối số tiền:`A * sum(i * s_{p[i]}) + B * sum(i)`Số hạng thứ hai chỉ phụ thuộc vào`N`, từ`sum(i)`được cố định như`N(N+1)/2`. Điều đó có nghĩa`B`hoàn toàn không ảnh hưởng đến việc đặt hàng; nó chỉ thay đổi giá trị cuối cùng. 

Vì vậy, toàn bộ tối ưu hóa giảm xuống tối đa hóa:`A * sum(i * s_{p[i]})`Bây giờ bài toán trở thành một nhiệm vụ sắp xếp thứ tự có trọng số cổ điển: cực đại hóa hoặc cực tiểu hóa tích số chấm giữa các vị trí và giá trị tùy thuộc vào dấu của`A`. 

Nếu như`A > 0`, chúng tôi muốn lớn`s`xuất hiện ở những vị trí lớn, vì cả hai`i`Và`s`đóng góp tích cực. Điều này phù hợp với bất đẳng thức sắp xếp lại: sắp xếp cả hai chuỗi theo cùng một thứ tự sẽ tối đa hóa tổng. 

Nếu như`A < 0`, chúng tôi thực sự muốn giảm thiểu`sum(i * s_{p[i]})`, vì vậy chúng tôi đảo ngược thứ tự: lớn`s`nên kết hợp với nhỏ`i`. 

Nếu như`A = 0`, toàn bộ số hạng đầu tiên biến mất và chỉ có hằng số`B * N(N+1)/2`vẫn giữ nguyên, nên mọi hoán vị đều tối ưu. 

Điều này biến vấn đề thành sắp xếp một lần và tính toán một đường tuyến tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N!) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(1) thêm (ngoài việc sắp xếp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính quy tắc so sánh chuyển đổi dựa trên dấu của`A`. Nếu như`A`là dương, chúng tôi muốn đặt nhỏ hơn`s`sớm hơn; nếu như`A`là số âm, chúng tôi muốn lớn hơn`s`trước đó. Nếu như`A`bằng 0, thứ tự không có ý nghĩa. 
2. Sắp xếp mảng giá trị độ cay theo quy tắc này. Điều này đảm bảo việc ghép nối giữa các vị trí và giá trị là tối ưu trong bất đẳng thức sắp xếp lại. 
3. Tính phần đóng góp không đổi từ`B`. Vì mọi vị trí`i`xuất hiện đúng một lần thì tổng của tất cả các trọng số vị trí là`N(N+1)/2`, do đó tổng đóng góp từ`B`đã được sửa. 
4. Tính số hạng chính bằng cách lặp qua mảng đã được sắp xếp. Duy trì chỉ số vị trí đang chạy`i + 1`và tích lũy`(i + 1) * A * s[i]`. 
5. Cộng hằng số`B * N(N+1)/2`đến kết quả tích lũy và xuất ra nó. 

### Tại sao nó hoạt động 

Mục tiêu tách biệt rõ ràng thành hàm tuyến tính phụ thuộc vào vị trí được áp dụng cho hoán vị. Sau khi được mở rộng, thuật ngữ duy nhất bị ảnh hưởng bởi thứ tự là`sum(i * s_{p[i]})`. Bất đẳng thức sắp xếp lại đảm bảo rằng tổng này được tối đa hóa bằng cách sắp xếp một chuỗi theo cùng thứ tự với chuỗi kia khi hệ số dương và theo thứ tự ngược lại khi âm. Từ`B`đóng góp một hằng số cố định độc lập với hoán vị, nó không ảnh hưởng đến sự sắp xếp tối ưu. Tính bất biến này đảm bảo rằng bất kỳ sai lệch nào so với cấu trúc đã sắp xếp chỉ có thể làm xấu đi tổng trọng số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, A, B = map(int, input().split())
    s = list(map(int, input().split()))
    
    if A == 0:
        # all permutations equal
        return B * N * (N + 1) // 2

    s.sort()

    if A < 0:
        s.reverse()

    res = 0
    for i, val in enumerate(s, 1):
        res += i * (A * val)

    res += B * N * (N + 1) // 2
    return res

if __name__ == "__main__":
    print(solve())
```Mã bắt đầu bằng cách đọc đầu vào và xử lý trường hợp suy biến trong đó`A = 0`, kể từ đó việc sắp xếp không có hiệu lực và câu trả lời thu về một tổng số học không đổi. 

Danh sách được sắp xếp theo thứ tự tăng dần theo mặc định và đảo ngược khi`A`là âm để đảm bảo ghép nối chính xác giữa các chỉ số vị trí và giá trị được chuyển đổi. Điều này trực tiếp thực hiện sự liên kết tối ưu được quyết định bởi sự bất đẳng thức sắp xếp lại. 

Vòng lặp tính toán sự đóng góp của`A`chỉ thuật ngữ, nhân mỗi giá trị với chỉ số dựa trên 1 của nó. các`B`số hạng được thêm một lần vào cuối bằng cách sử dụng tổng dạng đóng của số hạng đầu tiên`N`số nguyên, tránh mọi sự phụ thuộc vào thứ tự. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 1 13
5 8 3
```Thứ tự sắp xếp là`[3, 5, 8]`từ`A > 0`. 

| Đặt
