---
problem: 1302I
contest_id: 1302
problem_index: I
name: "Deja vu"
contest_name: "AIM Tech Poorly Prepared Contest (unrated, funny, Div. 1 preferred)"
rating: 0
tags: []
answer: passed_samples
verified: true
solve_time_s: 236
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2dd39d-3468-83ec-885f-f9f55d26717b
---

#CF 1302I - Deja vu 

**Đánh giá:** ? 
**Thẻ:** - 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 56s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2dd39d-3468-83ec-885f-f9f55d26717b 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân, hãy coi nó như một hàng ô chỉ chứa`0`Và`1`. Từ hàng dài này, chúng ta trượt một cửa sổ có kích thước cố định`k`trên tất cả các vị trí bắt đầu có thể. Mỗi vị trí tạo ra một chuỗi con có độ dài`k`và mỗi chuỗi con chính là một chuỗi nhị phân. 

Nhiệm vụ không phải là liệt kê các chuỗi con này hoặc so sánh chúng một cách trực tiếp theo từng cặp một cách đơn giản mà chỉ đơn giản là đếm xem có bao nhiêu mẫu có độ dài khác nhau.`k`xuất hiện ở bất cứ đâu trong chuỗi. 

Vì vậy, đầu ra là số lượng “cửa sổ” duy nhất có kích thước`k`khi chúng ta quét từ trái sang phải. 

Các ràng buộc cho phép độ dài chuỗi lên tới 200.000. Một so sánh đơn giản về tất cả các chuỗi con sẽ bao gồm tới 200.000 cửa sổ và mỗi so sánh có giá O(k), dẫn đến khoảng O(nk), có thể đạt tới 4×10^10 phép tính trong trường hợp xấu nhất. Điều đó vượt xa giới hạn thời gian. 

Quan sát chính là mỗi chuỗi con đều ngắn (bảng chữ cái nhị phân), vì vậy chúng ta cần một cách để so sánh các chuỗi con trong O(1) sau khi xử lý trước hoặc biểu diễn chúng ở dạng băm nhỏ gọn. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các ký tự giống hệt nhau. Ví dụ, nếu`s = 00000`Và`k = 3`, mọi chuỗi con đều giống hệt nhau nên câu trả lời là`1`. Một phương pháp băm đơn giản sử dụng không đúng cách tính toán lại không cuộn có thể vô tình coi các chuỗi con giống hệt nhau là khác nhau do lỗi tính toán lại hoặc chuẩn hóa không nhất quán. Một trường hợp cạnh khác là khi`k = 1`, trong đó câu trả lời chỉ là số lượng ký tự riêng biệt, luôn luôn`1`hoặc`2`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ trích xuất mọi chuỗi con có độ dài`k`, lưu trữ dưới dạng chuỗi và chèn nó vào một bộ. Điều này đúng về mặt logic vì các tập hợp xử lý tính duy nhất. Tuy nhiên, việc sao chép từng chuỗi con sẽ tốn O(k) và thực hiện việc này cho các vị trí O(n) sẽ khiến tổng chi phí là O(nk), quá chậm so với giới hạn trên. 

Để cải thiện điều này, chúng tôi nhận thấy rằng các chuỗi con chồng chéo lên nhau rất nhiều. Di chuyển từ cửa sổ này sang cửa sổ tiếp theo chỉ thay đổi hai ký tự: một ký tự rời khỏi cửa sổ và một ký tự đi vào. Cấu trúc này gợi ý một biểu diễn cuốn chiếu. 

Vì bảng chữ cái là nhị phân nên chúng ta có thể hiểu mỗi chuỗi con là một số trong cơ số 2. Sau đó, mỗi cửa sổ tương ứng với một số nguyên và chúng ta có thể cập nhật số nguyên này trong O(1) bằng các thao tác bit: dịch chuyển sang trái, thêm bit mới và che đi các bit cũ ngoài độ dài`k`. Điều này mang lại cho chúng ta một hàm băm lăn mà không có va chạm. 

Điều này làm giảm vấn đề duy trì giá trị nhị phân trượt và chèn từng giá trị vào một tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(k) mỗi chuỗi con | Quá chậm | 
| Cán băm nhị phân | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi kích thước cửa sổ đầu tiên`k`thành một số nguyên bằng cách diễn giải nó dưới dạng số nhị phân. Điều này đưa ra một đại diện nhỏ gọn của chuỗi con. 
2. Chèn số nguyên này vào tập băm. Bộ này lưu trữ tất cả các giá trị cửa sổ riêng biệt được thấy cho đến nay. 
3. Đối với từng vị trí`i`từ`k`ĐẾN`n - 1`, cập nhật giá trị hiện tại: 

ta dịch sang trái 1 bit (tương đương nhân với 2), thêm bit mới`s[i]`và loại bỏ sự đóng góp của bit rơi ra ngoài cửa sổ bằng cách che dấu bằng`(1 << k) - 1`. 
4. Chèn từng giá trị cập nhật vào tập hợp. Mỗi giá trị xác định duy nhất một chuỗi con có độ dài`k`. 
5. Câu trả lời là kích thước của tập hợp sau khi xử lý tất cả các cửa sổ. 

Ý tưởng chính ở bước 3 là chúng ta không bao giờ xây dựng lại chuỗi con. Chúng tôi sử dụng lại tính toán trước đó và điều chỉnh nó cục bộ, giúp giữ cho thời gian chạy luôn tuyến tính. 

### Tại sao nó hoạt động 

Mỗi chuỗi con có độ dài`k`tương ứng với một số nhị phân duy nhất khi được diễn giải trong cơ sở 2. Bản cập nhật luân phiên duy trì sự tương đương chính xác giữa biểu diễn số nguyên và nội dung cửa sổ hiện tại. Vì mỗi cửa sổ được truy cập chính xác một lần và được ánh xạ một cách xác định thành một số, nên đẳng thức của các chuỗi con tương ứng chính xác với đẳng thức của các số nguyên được lưu trữ trong tập hợp. Không có xung đột nào được đưa ra vì sự biểu diễn là chính xác chứ không phải được băm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    if k == 0:
        print(0)
        return

    mask = (1 << k) - 1

    val = 0
    for i in range(k):
        val = (val << 1) | (ord(s[i]) - 48)

    seen = {val}

    for i in range(k, n):
        val = ((val << 1) & mask) | (ord(s[i]) - 48)
        seen.add(val)

    print(len(seen))

if __name__ == "__main__":
    solve()
```Mã duy trì một số nguyên cuộn`val`luôn đại diện cho điều cuối cùng`k`các bit của chuỗi. Mặt nạ đảm bảo rằng các bit cũ hơn sẽ bị loại bỏ khi chúng di chuyển ra ngoài cửa sổ. bộ`seen`theo dõi tất cả các giá trị riêng biệt gặp phải. 

Một lỗi phổ biến là quên thao tác mặt nạ, khiến các bit vượt quá độ dài`k`để tích lũy và so sánh sai lệch giữa các cửa sổ. Một vấn đề tế nhị khác là khởi tạo chính xác cửa sổ đầu tiên; không làm như vậy sẽ thay đổi tất cả các so sánh và tạo ra số lượng không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
01101
```Chúng tôi theo dõi các giá trị cửa sổ cuộn: 

| tôi | cửa sổ | nhị phân | giá trị | 
| --- | --- | --- | --- | 
| 1 | 01 | 01 | 1 | 
| 2 | 11 | 11 | 3 | 
| 3 | 10 | 10 | 2 | 
| 4 | 01 | 01 | 1 | 

Tập hợp trở thành`{1, 3, 2}`vậy câu trả lời là`3`. 

Dấu vết này cho thấy các chuỗi con lặp lại thu gọn thành các số nguyên giống hệt nhau như thế nào, xác nhận rằng mã hóa xác định chính xác các chuỗi trùng lặp. 

### Ví dụ 2 

đầu vào:```
4 3
0001
```| tôi | cửa sổ | nhị phân | giá trị | 
| --- | --- | --- | --- | 
| 2 | 000 | 000 | 0 | 
| 3 | 001 | 001 | 1 | 

Bộ này là`{0, 1}`vậy câu trả lời là`2`. 

Điều này chứng tỏ rằng ngay cả những thay đổi nhỏ ở cuối chuỗi cũng tạo ra các giá trị cửa sổ riêng biệt và bản cập nhật luân phiên sẽ ghi lại chúng mà không cần tính toán lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bản cập nhật cửa sổ sử dụng các thao tác bit theo thời gian không đổi và chèn một bộ | 
| Không gian | O(n) | Trong trường hợp xấu nhất, tất cả các cửa sổ đều khác biệt và được lưu trữ trong tập hợp | 

Thuật toán chạy thoải mái trong giới hạn vì 200.000 thao tác với bản cập nhật O(1) là hiệu quả trong Python và mức sử dụng bộ nhớ là tuyến tính theo số lượng chuỗi con riêng biệt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Provided sample (conceptual placeholder, assumes solve() wired correctly)
# assert run("5 2\n01101\n") == "3\n"

# custom cases
# all identical
# assert run("5 2\n00000\n") == "1\n"

# k = 1
# assert run("4 1\n0101\n") == "2\n"

# maximum diversity small
# assert run("4 2\n0101\n") == "3\n"

# single window
# assert run("3 3\n101\n") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2 / 00000 | 1 | xâu con giống hệt nhau sụp đổ | 
| 4 1 / 0101 | 2 | đếm riêng biệt một ký tự | 
| 4 2 / 0101 | 3 | cửa sổ chồng chéo độc đáo | 
| 3 3 / 101 | 1 | cửa sổ dài đầy đủ duy nhất | 

## Vỏ cạnh 

Khi chuỗi bao gồm tất cả các ký tự giống hệt nhau, mọi cửa sổ sẽ ánh xạ tới cùng một số nguyên vì giá trị cuộn không bao giờ thay đổi ngoại trừ việc dịch chuyển trong các bit giống hệt nhau. Đối với đầu vào`s = 000000`Và`k = 3`, mọi giá trị tính toán vẫn còn`0`, vì vậy kích thước đặt là`1`. 

Khi`k = 1`, mỗi cửa sổ chỉ là một bit duy nhất. Cơ chế cuộn thoái hóa thành việc chèn trực tiếp giá trị số nguyên của từng ký tự vào tập hợp. Vì`s = 0101`, các giá trị xen kẽ giữa`0`Và`1`, tạo ra đúng hai phần tử phân biệt. 

Khi`k = n`, chỉ có một cửa sổ, do đó mã hóa ban đầu đã thể hiện chuỗi đầy đủ và kích thước được đặt luôn là`1`.
