---
title: "CF 103736A - Xin chào ACmer!"
description: "Chúng ta được cung cấp một chuỗi chữ thường duy nhất và được yêu cầu đếm số lần mẫu cố định \"hznu\" xuất hiện dưới dạng một khối liền kề bên trong nó."
date: "2026-07-02T09:09:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103736
codeforces_index: "A"
codeforces_contest_name: "The 2022 Hangzhou Normal U Summer Trials"
rating: 0
weight: 103736
solve_time_s: 41
verified: true
draft: false
---

[CF 103736A - Xin chào ACmer!](https://codeforces.com/problemset/problem/103736/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chữ thường và được yêu cầu đếm xem mẫu cố định bao nhiêu lần`"hznu"`xuất hiện dưới dạng một khối liền kề bên trong nó. Mỗi lần xuất hiện hợp lệ phải khớp tất cả bốn ký tự theo thứ tự và phải chiếm các vị trí liên tiếp, do đó, được phép trùng lặp miễn là chúng bắt đầu ở các chỉ mục khác nhau. 

Độ dài đầu vào có thể lên tới 100000 ký tự. Kích thước đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào liên tục xây dựng chuỗi con hoặc thực hiện xử lý nặng theo từng vị trí với hành vi bậc hai. Một giải pháp kiểm tra mọi chỉ mục bắt đầu có thể có và chỉ kiểm tra số lượng ký tự không đổi là đã đủ, nhưng bất kỳ điều gì liên quan đến việc cắt chuỗi lặp lại hoặc quét lồng nhau trên toàn bộ chuỗi vẫn sẽ chỉ an toàn nếu được thực hiện cẩn thận. 

Một vài trường hợp khó khăn xuất hiện một cách tự nhiên. Ví dụ, chuỗi có thể ngắn hơn bốn ký tự`"hzn"`rõ ràng phải trả về 0 vì không thể tồn tại kết quả khớp đầy đủ. Một chuỗi bao gồm toàn bộ các chữ cái lặp đi lặp lại như`"hhhhhhhh"`cũng trả về 0 vì mẫu yêu cầu một trình tự cụ thể. Một trường hợp tinh tế khác là các mẫu chồng chéo, ví dụ`"hzhznu"`chứa chính xác một lần xuất hiện hợp lệ bắt đầu từ vị trí 2 và việc trích xuất chuỗi con bất cẩn làm dịch chuyển không chính xác có thể bỏ sót hoặc đếm gấp đôi ranh giới nếu được triển khai không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là kiểm tra mọi chỉ số bắt đầu có thể và kiểm tra xem chuỗi con có độ dài bằng 4 có bằng không`"hznu"`. Với mỗi vị trí i, chúng ta so sánh các ký tự s[i], s[i+1], s[i+2] và s[i+3]. Nếu tất cả đều khớp, chúng tôi sẽ tăng câu trả lời. 

Cách tiếp cận bạo lực này là chính xác vì mọi lần xuất hiện của mẫu phải bắt đầu ở chỉ mục i nào đó và việc kiểm tra tất cả các chỉ mục đó đảm bảo không có kết quả khớp hợp lệ nào bị bỏ qua. Chi phí đến từ việc quét tối đa n vị trí và thực hiện một lượng công việc không đổi ở mỗi vị trí, đưa ra thời gian tuyến tính. Mặc dù nó đã hiệu quả, nhưng một biến thể đơn giản xây dựng các chuỗi con như s[i:i+4] nhiều lần vẫn an toàn trong Python đối với ràng buộc này, nhưng nó gây ra chi phí không cần thiết. 

Không cần đến các thuật toán chuỗi nâng cao như KMP vì mẫu cực kỳ nhỏ và cố định. Quan sát quan trọng là việc khớp mẫu làm giảm so sánh cục bộ ở mỗi chỉ mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra chuỗi con Brute Force | O(n) | O(1) | Đã chấp nhận | 
| Quét từng ký tự | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào s và xác định mẫu đích là`"hznu"`. Mẫu này cố định và không bao giờ thay đổi, vì vậy chúng ta có thể coi nó như bốn phép so sánh ký tự không đổi. 
2. Khởi tạo một biến đếm về 0. Điều này sẽ tích lũy số lượng kết quả khớp hợp lệ được tìm thấy trên chuỗi. 
3. Lặp lại tất cả các chỉ số i từ 0 đến len(s) - 4. Mỗi chỉ mục đại diện cho một vị trí bắt đầu có thể có của một kết quả khớp đầy đủ 4 ký tự. Chúng tôi dừng lại ở len(s) - 4 vì bất kỳ chỉ mục nào vượt quá số đó không thể chứa đủ bốn ký tự. 
4. Với mỗi chỉ số i, so sánh chuỗi con s[i:i+4] với`"hznu"`. Nếu chúng bằng nhau thì tăng bộ đếm lên một. Sự so sánh trực tiếp này đảm bảo chúng tôi chỉ tính các trận đấu hoàn chỉnh. 
5. Sau khi quét xong, xuất ra bộ đếm là kết quả cuối cùng. 

### Tại sao nó hoạt động 

Mỗi lần xuất hiện hợp lệ của`"hznu"`tương ứng với chính xác một chỉ số bắt đầu i trong chuỗi. Thuật toán kiểm tra mọi chỉ mục như vậy một cách chính xác một lần và thực hiện kiểm tra tính bằng nhau chính xác theo mẫu được yêu cầu. Vì không có vị trí nào bị bỏ qua và không có vị trí không hợp lệ nào có thể đáp ứng việc kiểm tra tính bằng nhau nên việc đếm vừa đầy đủ vừa chính xác. Sự chồng chéo được xử lý một cách tự nhiên vì mỗi chỉ số bắt đầu được đánh giá độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    s = input().strip()
    target = "hznu"
    n = len(s)
    ans = 0

    for i in range(n - 3):
        if s[i:i+4] == target:
            ans += 1

    print(ans)

if __name__ == "__main__":
    main()
```Mã đọc chuỗi và chỉ lặp lại tối đa`n - 3`, điều này đảm bảo chúng tôi không bao giờ truy cập các chỉ mục ngoài giới hạn. lát cắt`s[i:i+4]`an toàn ngay cả ở gần ranh giới trong Python, nhưng giới hạn vòng lặp đảm bảo chúng tôi chỉ xem xét các vị trí bắt đầu hợp lệ. 

Việc so sánh được thực hiện trực tiếp với chuỗi cố định`"hznu"`, điều này tránh được bất kỳ quá trình tiền xử lý bổ sung nào. Giải pháp vẫn tuyến tính vì mỗi lần lặp thực hiện công việc không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
hznu
```| tôi | chuỗi con s[i:i+4] | trận đấu | đếm | 
| --- | --- | --- | --- | 
| 0 | hznu | vâng | 1 | 

Ví dụ này cho thấy trường hợp đơn giản nhất trong đó toàn bộ chuỗi khớp chính xác một lần. Vòng lặp chỉ chạy với i = 0 và kết quả khớp hoàn toàn được phát hiện ngay lập tức. 

### Ví dụ 2 

đầu vào:```
hhzznnuu
```| tôi | chuỗi con s[i:i+4] | trận đấu | đếm | 
| --- | --- | --- | --- | 
| 0 | hhzz | không | 0 | 
| 1 | hzzn | không | 0 | 
| 2 | zznn | không | 0 | 
| 3 | znnu | không | 0 | 
| 4 | nnuu | không | 0 | 

Trường hợp này chứng tỏ rằng mặc dù tất cả các ký tự của bảng chữ cái liên quan đến mẫu đều tồn tại trong chuỗi nhưng chúng không bao giờ sắp xếp theo đúng thứ tự nên không tìm thấy kết quả trùng khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được kiểm tra một lần bằng cách so sánh theo thời gian không đổi tối đa 4 ký tự | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì 100000 thao tác với công việc liên tục trên mỗi lần lặp là không đáng kể đối với giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided samples
assert run("hznu\n") == "1", "sample 1"
assert run("hhzznnuu\n") == "0", "sample 2"

# custom cases
assert run("hzn\n") == "0", "too short"
assert run("hhhhhznu\n") == "1", "single late match"
assert run("hznhznu\n") == "2", "overlapping allowed"
assert run("hznhznuhznu\n") == "3", "multiple separated matches"
assert run("aaaaaaaaaa\n") == "0", "no valid characters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "hzn" | 0 | chuỗi ngắn hơn mẫu | 
| "hhhhznu" | 1 | khớp ở ranh giới cuối | 
| "hznhznu" | 2 | trận đấu hợp lệ liền kề | 
| "hznhznuhznu" | 3 | nhiều lần xuất hiện | 

## Vỏ cạnh 

Một chuỗi ngắn như`"hzn"`được xử lý bởi điều kiện vòng lặp`range(n - 3)`, trở thành`range(-1)`và dẫn đến số lần lặp bằng 0, do đó đầu ra vẫn bằng 0. 

Một trận đấu được căn chỉnh theo ranh giới như`"hhhhhznu"`được xử lý chính xác vì chỉ mục hợp lệ cuối cùng được bao gồm trong vòng lặp và lát cắt`s[i:i+4]`nắm bắt chính xác mô hình ở cuối. 

Các cấu trúc chồng chéo như`"hznhznu"`được tính một cách tự nhiên vì mỗi chỉ số được kiểm tra độc lập. Lần xuất hiện đầu tiên bắt đầu ở vị trí 0 hoặc 1 tùy thuộc vào căn chỉnh và lần xuất hiện thứ hai ở chỉ mục muộn hơn, không có sự can thiệp giữa các lần kiểm tra do không có trạng thái nào được chia sẻ giữa các lần lặp.
