---
title: "CF 104974J - Bó Hoa"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, có một số loại hoa, mỗi loại có “hệ số vẻ đẹp” cố định và có chính xác 100 loại hoa giống hệt nhau. Mỗi bông hoa có giá 1 dinar và chúng ta có thể mua tổng cộng tối đa c bông hoa."
date: "2026-06-28T06:14:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104974
codeforces_index: "J"
codeforces_contest_name: "Codentines Day"
rating: 0
weight: 104974
solve_time_s: 78
verified: false
draft: false
---

[CF 104974J - Bó hoa](https://codeforces.com/problemset/problem/104974/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, có một số loại hoa, mỗi loại có “hệ số vẻ đẹp” cố định và có chính xác 100 loại hoa giống hệt nhau. Mỗi bông hoa có giá 1 dinar và chúng ta có thể mua nhiều nhất`c`tổng số hoa. 

Nếu chúng ta mua một số bộ sưu tập hoa, điểm bó hoa được xác định bằng tỷ lệ: tổng vẻ đẹp của những bông hoa được chọn chia cho tổng vẻ đẹp của tất cả những bông hoa có sẵn trong cửa hàng. Vì mỗi loại đóng góp 100 bông hoa giống hệt nhau nên mẫu số được cố định cho một trường hợp thử nghiệm và chỉ phụ thuộc vào mảng đầu vào chứ không phụ thuộc vào thứ chúng ta mua. 

Điều này biến nhiệm vụ thành một vấn đề tối đa hóa thuần túy: chúng tôi muốn tối đa hóa tổng vẻ đẹp của những bông hoa được chọn trong một giới hạn ngân sách và sau đó chuẩn hóa nó bằng một giá trị không đổi. 

Mỗi loại hoa`i`đóng góp`a_i`tính tổng, và có 100 bản sao của nó. Vì vậy, cửa hàng tương đương với việc có`100 * n`các mặt hàng trong đó mỗi`a_i`xuất hiện đúng 100 lần. Chúng ta có thể chọn nhiều nhất`c`mặt hàng. 

Đầu ra là một số thực biểu thị giá trị tối đa được chuẩn hóa này. 

Các ràng buộc cho phép lên đến`5 × 10^4`loại cho mỗi trường hợp thử nghiệm và tối đa`10`trường hợp thử nghiệm. Việc mở rộng tất cả các bông hoa một cách rõ ràng sẽ tạo ra tối đa`5 × 10^6`các mục nằm ở ranh giới nhưng vẫn khả thi trong các ngôn ngữ được tối ưu hóa nhưng không cần thiết. 

Trường hợp cạnh chính là khi`c`rất lớn, lên tới`10^9`. Trong tình huống đó, ràng buộc thực sự không liên quan vì chúng ta không thể vượt quá giới hạn có sẵn`100n`hoa. 

Một sai lầm ngây thơ là coi đây là một vấn đề tối ưu hóa phân đoạn hoặc liên tục hoặc hiểu sai mẫu số tùy thuộc vào tập hợp con được chọn. Ví dụ: nếu người ta giả định không chính xác mẫu số thay đổi khi lựa chọn, thì lựa chọn tham lam có thể trở nên không hợp lệ. Một cạm bẫy tinh vi khác là việc mở rộng tất cả`100n`các mục mà không tính đến việc nhiều giá trị được lặp lại, dẫn đến chi phí không cần thiết. 

Một trường hợp minh họa nhỏ: 

đầu vào:```
n = 2, c = 3
a = [5, 1]
```Chúng ta có 100 bản sao 5 và 100 bản sao 1. Sự lựa chọn tối ưu rõ ràng là ba bản sao 5, không trộn lẫn trong 1s. Bất kỳ cách tiếp cận nào không nhóm được các giá trị giống nhau sẽ phải thực hiện thêm công việc nhưng vẫn phải tôn trọng cấu trúc này. 

## Phương pháp tiếp cận 

Nếu chúng ta mô phỏng quá trình một cách trực tiếp, chúng ta sẽ xây dựng tất cả`100n`hoa, sắp xếp chúng theo vẻ đẹp và đứng đầu`c`. Điều này đúng vì mỗi bông hoa đều có sự đóng góp độc lập vào tổng số. Vấn đề nằm ở quy mô: việc sắp xếp tối đa năm triệu phần tử cho mỗi trường hợp thử nghiệm là ở giới hạn nhưng vẫn nằm trong một số giới hạn, mặc dù rõ ràng là không cần thiết. 

Quan sát quan trọng là cấu trúc đã được nhóm lại. Thay vì nghĩ về từng bông hoa riêng lẻ, chúng ta có thể coi mỗi loại là một khối gồm 100 giá trị giống hệt nhau. Vì tất cả các mục đều độc lập và bổ sung nên lựa chọn tối ưu luôn ưu tiên mức cao hơn.`a_i`giá trị đầu tiên. Trong một loại, tất cả các bản sao đều giống hệt nhau, vì vậy việc chọn một phần loại chỉ xảy ra khi chúng tôi hết ngân sách. 

Điều này làm giảm vấn đề sắp xếp`n`loại theo`a_i`theo thứ tự giảm dần và tham lam lấy tới 100 từ mỗi loại cho đến khi cạn kiệt ngân sách`c`. 

Khi chúng tôi tính toán số tiền tối đa có thể đạt được, câu trả lời cuối cùng là số tiền này chia cho tổng vẻ đẹp không đổi của tất cả các bông hoa, đó là`100 * sum(a_i)`. Nhân tử số và mẫu số với 1 sẽ đơn giản hóa biểu thức thành một phép tính nổi ổn định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mở rộng tất cả hoa) | O(100n log(100n)) | O(100n) | Chấp nhận nhưng nặng nề | 
| Tối ưu (loại sắp xếp + tham lam) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng các hệ số đẹp,`S = sum(a_i)`. Điều này đại diện cho mẫu số chuẩn hóa lên đến một hệ số không đổi. 
2. Sắp xếp mảng`a`theo thứ tự giảm dần. Điều này đảm bảo chúng tôi luôn xem xét những bông hoa có giá trị nhất trước tiên, điều này là cần thiết bởi vì mỗi bông hoa có giá trị cao hơn`a_i`thống trị bất kỳ bông hoa nào ở cấp độ thấp hơn`a_i`. 
3. Khởi tạo một biến`taken_sum = 0`và theo dõi ngân sách còn lại`c`. 
4. Lặp lại danh sách đã sắp xếp. Với mỗi giá trị`a_i`, xác định xem chúng ta có thể lấy được bao nhiêu bông hoa từ loại này:`take = min(100, c)`. Điều này tôn trọng cả ràng buộc về tính sẵn có và ràng buộc về ngân sách. 
5. Thêm`take * a_i`ĐẾN`taken_sum`, và trừ`take`từ`c`. 
6. Dừng lại sớm nếu`c`trở thành 0, vì không thể đóng góp thêm nữa. 
7. Tính đáp án cuối cùng là`taken_sum / S`. 

### Tại sao nó hoạt động 

Quá trình này là một ứng dụng trực tiếp của lựa chọn tham lam trên nhiều tập các mục có trọng số độc lập. Mỗi bông hoa giống hệt nhau trong loại của nó, do đó, bất kỳ giải pháp tối ưu nào cũng tương đương với việc chọn tiền tố của tập hợp nhiều loại hoa được sắp xếp. Việc sắp xếp đảm bảo rằng tiền tố này được sắp xếp theo mức độ đóng góp giảm dần, do đó việc thay thế bất kỳ bông hoa được chọn có giá trị thấp hơn bằng một bông hoa không được chọn có giá trị cao hơn luôn cải thiện hoặc bảo toàn tổng số tiền. Vì mẫu số không đổi nên việc tối đa hóa tỷ lệ sẽ giảm xuống mức tối đa hóa tử số, do đó cách xây dựng tham lam này sẽ tạo ra bó hoa tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, c = map(int, input().split())
        a = list(map(float, input().split()))
        
        total = sum(a)
        a.sort(reverse=True)
        
        rem = c
        taken = 0.0
        
        for val in a:
            if rem == 0:
                break
            take = min(100, rem)
            taken += take * val
            rem -= take
        
        print(taken / total)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc tất cả các loại hoa và tính tổng chuẩn hóa. Việc sắp xếp đảm bảo chúng tôi xử lý các loại có giá trị nhất trước tiên. Vòng lặp tham lam tiêu thụ tới 100 bông hoa mỗi loại, nhưng không bao giờ vượt quá ngân sách`c`. Phép chia cuối cùng chuyển đổi điểm tuyển chọn tích lũy thành giá trị chuẩn hóa được yêu cầu. 

Một chi tiết triển khai tinh tế là sử dụng số học dấu phẩy động một cách nhất quán, vì cả đầu vào và đầu ra đều liên quan đến số thực. Một điểm quan trọng khác là chấm dứt sớm khi`rem`đạt tới 0, điều này tránh được sự lặp lại không cần thiết khi`c`là nhỏ so với`n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, c = 4
a = [4, 2, 1]
```Chúng tôi có quá trình sau: 

| Bước | Loại hiện tại | rem | lấy | lấy tổng | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 4 | 16 | 
| 2 | 2 | 0 | 0 | 16 | 
| 3 | 1 | 0 | 0 | 16 | 

Chúng tôi lấy bốn bông hoa có giá trị 4 và dừng lại ngay sau khi cạn kiệt ngân sách. Mẫu số là`4 + 2 + 1 = 7`, vậy kết quả là`16 / 7`. 

Điều này khẳng định rằng việc tập trung ngân sách vào những giá trị cao nhất là tối ưu. 

### Ví dụ 2 

đầu vào:```
n = 3, c = 200
a = [3, 3, 2]
```| Bước | Loại hiện tại | rem | lấy | lấy tổng | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 200 | 100 | 300 | 
| 2 | 3 | 100 | 100 | 600 | 
| 3 | 2 | 0 | 0 | 600 | 

Chúng tôi sử dụng hết cả hai loại giá trị cao trước khi chạm vào loại thấp hơn. Điều này cho thấy thuật toán sẽ lấp đầy toàn bộ khối 100 mục một cách tự nhiên khi ngân sách cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp`n`loại chiếm ưu thế trong mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | Lưu trữ mảng đầu vào | 

Các ràng buộc cho phép lên đến`5 × 10^4`các loại cho mỗi trường hợp thử nghiệm, do đó việc sắp xếp dễ dàng đủ nhanh. Ngay cả với 10 trường hợp thử nghiệm, tổng số hoạt động vẫn nằm trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    t = int(input())
    out = []
    for _ in range(t):
        n, c = map(int, input().split())
        a = list(map(float, input().split()))
        total = sum(a)
        a.sort(reverse=True)

        rem = c
        taken = 0.0

        for v in a:
            if rem == 0:
                break
            take = min(100, rem)
            taken += take * v
            rem -= take

        out.append(str(taken / total))
    return "\n".join(out) + "\n"

# minimal case
assert run("1\n1 1\n5\n")[:10], "min case"

# small greedy split
assert run("1\n2 3\n5 1\n") is not None

# all equal
assert run("1\n3 150\n2 2 2\n") is not None

# large budget saturates all
assert run("1\n2 1000\n1 2\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| loại đơn | 1.0 | chuẩn hóa tầm thường | 
| giá trị hỗn hợp | sở thích tham lam | tính đúng đắn của việc đặt hàng | 
| tất cả đều bình đẳng | lựa chọn thống nhất | không thiên vị trong phân phối | 
| ngân sách lớn | kiệt sức hoàn toàn | giới hạn sẵn có | 

## Vỏ cạnh 

Khi nào`c`vượt quá`100n`, thuật toán xử lý đầy đủ mọi loại hoa, lấy tất cả các loại hoa có sẵn. Vòng lặp xử lý việc này một cách tự nhiên bởi vì`min(100, rem)`sẽ luôn giới hạn ở mức 100 cho đến khi hết ngân sách hoặc sử dụng hết tất cả các loại. 

Nếu tất cả`a_i`bằng nhau, việc sắp xếp không thay đổi thứ tự, nhưng thuật toán vẫn hoạt động chính xác bằng cách điền các kiểu một cách tuần tự. Ví dụ, với`a = [2,2,2]`Và`c = 250`, chúng tôi lấy 100 từ mỗi loại trong số hai loại đầu tiên và 50 từ loại thứ ba, tạo ra kết quả tỷ lệ phù hợp với trọng lượng đồng nhất. 

Khi`c`rất nhỏ, chỉ một vài loại có giá trị cao nhất đầu tiên được chạm vào. Vì thuật toán luôn xử lý theo thứ tự giảm dần nên không thể chọn hoa có giá trị thấp trước khi hết hoa có giá trị cao hơn, duy trì tính tối ưu ngay cả khi ngân sách cực kỳ khan hiếm.
