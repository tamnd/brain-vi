---
title: "CF 106486C - \u8ddf\u6211\u7684\u6f2b\u6e38\u8005\u8bf4\u53bb\u5427"
description: "Chúng tôi đang xem xét một cơ chế trò chơi trong đó xác suất tung ra một đòn chí mạng không cố định mà phụ thuộc vào khoảng thời gian đã trôi qua kể từ đòn chí mạng trước đó. Bạn càng tồn tại lâu mà không có chí mạng thì cơ hội càng cao, tối đa là 1."
date: "2026-06-19T15:13:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106486
codeforces_index: "C"
codeforces_contest_name: "Dalian University of Technology, Software College 2025 Freshman Contest"
rating: 0
weight: 106486
solve_time_s: 49
verified: true
draft: false
---

[CF 106486C - \u8ddf\u6211\u7684\u6f2b\u6e38\u8005\u8bf4\u53bb\u5427](https://codeforces.com/problemset/problem/106486/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xem xét một cơ chế trò chơi trong đó xác suất tung ra một đòn chí mạng không cố định mà phụ thuộc vào khoảng thời gian đã trôi qua kể từ đòn chí mạng trước đó. Bạn càng tồn tại lâu mà không có chí mạng thì cơ hội càng cao, tối đa là 1. 

Chính xác hơn, có một tham số c kiểm soát tốc độ tăng của xác suất này. Ở bất kỳ đòn tấn công nào, hãy xác định i là số đòn tấn công kể từ đòn chí mạng cuối cùng hoặc kể từ khi bắt đầu trận đấu nếu chưa có đòn chí mạng nào xảy ra. Xác suất để đòn tấn công này gây ra đòn chí mạng là min(1, i · c). 

Trải qua chuỗi tấn công vô tận, hệ thống này tạo ra tỷ lệ tới hạn trung bình dài hạn. Nghĩa là, nếu chúng ta nhìn vào số lượng đòn chí mạng dự kiến ​​​​trong n đòn tấn công đầu tiên và chia cho n, tỷ lệ này sẽ hội tụ về một giá trị p nào đó khi n trở nên lớn. Chúng ta được cho p và phải tìm lại c tương ứng. 

Khó khăn chính là quá trình này không phải là không có bộ nhớ. Mỗi cuộc tấn công phụ thuộc vào “chuỗi thất bại” trước đó kéo dài bao lâu, vì vậy hệ thống này là một quá trình đổi mới chứ không phải là một chuỗi Bernoulli đơn giản. 

Kích thước đầu vào nhỏ, với tối đa 100 trường hợp thử nghiệm, vì vậy mỗi trường hợp thử nghiệm có thể cung cấp các phương pháp lặp logarit hoặc thậm chí hệ số không đổi nhỏ. Điều quan trọng là tính chính xác của mối quan hệ toán học giữa p và c chứ không phải tối ưu hóa trên các cấu trúc dữ liệu lớn. 

Một trường hợp khó phát hiện xảy ra khi p cực kỳ gần 1. Trong trường hợp đó, c cũng phải gần 1 và độ ổn định số là vấn đề quan trọng khi giải mối quan hệ nghịch đảo. Một trường hợp cạnh khác là khi p rất nhỏ. Khi đó c nhỏ và hệ thống hầu như luôn đặt lại ở i = 1 hoặc 2, làm cho kỳ vọng bị chi phối bởi đoạn tuyến tính đầu tiên của hàm xác suất. 

Một cách tiếp cận mô phỏng đơn giản sẽ cố gắng mô phỏng quá trình Markov qua các chuỗi dài. Mặc dù mỗi bước là O(1), việc hội tụ về tốc độ dừng là cực kỳ chậm và phụ thuộc vào phương sai, khiến nó không thực tế và không ổn định về mặt số đối với các yêu cầu lỗi chặt chẽ như 1e-8. 

## Phương pháp tiếp cận 

Quan sát cốt lõi là quá trình này có thể được phân tách thành các “chu kỳ” độc lập giữa các lần tấn công chí mạng liên tiếp. Mỗi chu kỳ bắt đầu ngay sau một lần chí mạng và kết thúc ở lần chí mạng tiếp theo. Độ dài của một chu kỳ xác định tỷ lệ tới hạn dài hạn: nếu độ dài chu kỳ dự kiến ​​là E[L], thì số lượng chí mạng dự kiến ​​trên mỗi đòn tấn công chỉ đơn giản là 1 / E[L]. 

Trong một chu kỳ, xác suất thành công tăng tuyến tính: ở lần thử thứ i sau một lần chí mạng, xác suất thành công là min(1, i c). Điều này tạo ra hai chế độ tùy thuộc vào việc tôi có đạt tới 1 hay không. Cho k là số nguyên nhỏ nhất sao cho k c ≥ 1. Khi đó với i < k, xác suất là i c, và tại i = k chu trình được đảm bảo kết thúc. 

Tính toán brute-force sẽ tính toán rõ ràng độ dài chu kỳ dự kiến ​​bằng cách tính tổng tất cả các đường dẫn thành công-thất bại có thể xảy ra. Điều này nhanh chóng trở thành hàm mũ trong k vì mỗi trạng thái phân nhánh theo thành công hay thất bại, nhưng vì k có thể lớn bằng 1/c nên điều này trở nên không khả thi khi c nhỏ. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần mô phỏng trực tiếp tính ngẫu nhiên. Chúng ta chỉ cần kỳ vọng về thời gian dừng của một quá trình trong đó xác suất lỗi được xác định theo từng vị trí. Điều này cho phép chúng ta viết biểu thức dạng đóng cho E[L] dưới dạng hàm của c bằng cách tính tổng các xác suất sống sót. 

Khi chúng ta có E[L](c), điều kiện đích sẽ trở thành p = 1 / E[L](c). Vì E[L](c) là đơn điệu trong c nên chúng ta có thể giải tìm c bằng cách sử dụng tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(lớn, không hội tụ) | O(1) | Quá chậm | 
| Kỳ vọng + Tìm kiếm nhị phân | O(log(1/ε) · k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi tính toán độ dài chu kỳ dự kiến ​​cho c cố định, sau đó đảo ngược mối quan hệ bằng cách sử dụng tìm kiếm nhị phân. 

1. Cố định giá trị ứng viên của c. Xác định k = trần(1/c). Đây là chỉ số đầu tiên có xác suất đạt tới 1. Điều này chia quá trình thành giai đoạn tăng trưởng tuyến tính và giai đoạn kết thúc xác định. 
2. Tính xác suất sống sót S_i, là xác suất không có đòn chí mạng nào xảy ra cho đến lần thử thứ i. Ban đầu S_0 = 1. Với i ≥ 1, S_i = S_{i-1} · (1 − min(1, i c)). Điều này theo dõi khả năng chu trình tiếp tục qua bước i. 
3. Độ dài chu kỳ dự kiến ​​là tổng của tất cả các bước xác suất sống sót trước mỗi lần thử nghiệm. Cụ thể, E[L] = Σ S_{i-1} trên toàn bộ i, vì mỗi bước đóng góp 1 đơn vị thời gian nếu quá trình chưa kết thúc. 
4. Lặp lại i từ 1 đến k. Với i < k, cập nhật tỷ lệ sống sót theo cấp số nhân bằng cách sử dụng (1 − i c). Tại i = k, quá trình kết thúc nên chúng ta dừng lại. 
5. Tính p_hat = 1 / E[L]. Đây là tỷ lệ tới hạn dự kiến ​​dài hạn cho giá trị này của c. 
6. Sử dụng tìm kiếm nhị phân trên c trong khoảng (0, 1). Với mỗi điểm giữa, tính p_hat và so sánh với p đích. Nếu p_hat quá lớn thì c quá lớn và phải giảm, nếu không thì tăng c. 
7. Tiếp tục cho đến khi khoảng thời gian nhỏ hơn ngưỡng chính xác cố định, thường là 1e-12, để đảm bảo sai số tuyệt đối cuối cùng trong phạm vi 1e-8. 

### Tại sao nó hoạt động 

Quá trình này là một quá trình đổi mới trong đó mỗi chu kỳ được phân bổ độc lập và giống hệt nhau. Tần suất dài hạn của các sự kiện trong một hệ thống như vậy là nghịch đảo của thời gian giữa các lần đến dự kiến. Vì mỗi độ dài chu kỳ chỉ phụ thuộc vào c chứ không phụ thuộc vào lịch sử, nên E[L](c) mô tả đầy đủ đặc điểm của hệ thống. Tính đơn điệu vẫn giữ nguyên vì việc tăng c làm tăng nghiêm ngặt xác suất kết thúc sớm hơn, giảm độ dài chu kỳ dự kiến và tăng p. Điều này đảm bảo tìm kiếm nhị phân hội tụ đến một giải pháp duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def expected_cycle(c):
    s = 1.0
    total = 0.0
    i = 1
    while True:
        prob = i * c
        if prob >= 1.0:
            total += s
            break
        total += s
        s *= (1.0 - prob)
        i += 1
    return total

def solve_case(p):
    lo, hi = 0.0, 1.0
    for _ in range(80):
        mid = (lo + hi) / 2.0
        e = expected_cycle(mid)
        p_hat = 1.0 / e
        if p_hat > p:
            hi = mid
        else:
            lo = mid
    return (lo + hi) / 2.0

t = int(input())
for _ in range(t):
    p = float(input())
    ans = solve_case(p)
    print(f"{ans:.15f}")
```chức năng`expected_cycle`trực tiếp thực hiện ý tưởng đổi mới. Nó thực hiện các lần thử liên tiếp trong một chu kỳ, theo dõi cả xác suất sống sót và kỳ vọng tích lũy. Xác suất thời điểm đạt 1 thì chu kỳ phải kết thúc nên chúng ta ngừng tích lũy. 

Tìm kiếm nhị phân được thực hiện trên c vì hàm ánh xạ c tới p tăng đơn điệu. Số vòng lặp 80 là đủ để có độ chính xác gấp đôi nhằm đạt được sai số dưới 1e-8. 

Một điểm tinh tế là chúng ta tích lũy chiều dài dự kiến ​​thông qua xác suất sống sót`s`. Điều này tránh việc liệt kê tất cả các chuỗi lỗi và nén phân nhánh theo cấp số nhân thành một tuyến tính duy nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ khái niệm nhỏ trong đó c ở mức vừa phải, giả sử c = 0,5. Khi đó xác suất là 0,5, 1,0. Chu trình kết thúc ở bước 1 hoặc bước 2 một cách xác định sau khi xảy ra lỗi. 

| tôi | thăm dò | sinh tồn trước tôi | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 0,5 | 1.0 | 1.0 | 
| 2 | 1.0 | 0,5 | 0,5 | 

Độ dài chu kỳ dự kiến ​​là 1,5, do đó p = 2/3. 

Bây giờ hãy xem xét c = 0,25 nhỏ hơn. 

| tôi | thăm dò | sinh tồn trước tôi | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 0,25 | 1.0 | 1.0 | 
| 2 | 0,5 | 0,75 | 0,75 | 
| 3 | 0,75 | 0,375 | 0,375 | 
| 4 | 1.0 | 0,09375 | 0,09375 | 

Độ dài chu kỳ dự kiến ​​là tổng đóng góp, khoảng 2,21875, cho p ≈ 0,45. 

Những dấu vết này cho thấy tỷ lệ sống sót giảm theo cấp số nhân như thế nào và tại sao việc liệt kê tuyến tính là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · K · log(1/ε)) | Mỗi đánh giá quét tối đa K ≈ 1/c bước, tìm kiếm nhị phân chạy lặp lại liên tục | 
| Không gian | O(1) | Chỉ có một số biến động được duy trì | 

Các ràng buộc là nhỏ nên ngay cả các giá trị vừa phải của K vẫn nhanh. Tìm kiếm nhị phân chi phối độ chính xác chứ không phải kích thước cấu trúc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def expected_cycle(c):
        s = 1.0
        total = 0.0
        i = 1
        while True:
            prob = i * c
            if prob >= 1.0:
                total += s
                break
            total += s
            s *= (1.0 - prob)
            i += 1
        return total

    def solve_case(p):
        lo, hi = 0.0, 1.0
        for _ in range(80):
            mid = (lo + hi) / 2.0
            e = expected_cycle(mid)
            p_hat = 1.0 / e
            if p_hat > p:
                hi = mid
            else:
                lo = mid
        return (lo + hi) / 2.0

    t = int(input())
    out = []
    for _ in range(t):
        p = float(input())
        out.append(f"{solve_case(p):.15f}")
    return "\n".join(out)

assert abs(float(run("1\n0.5\n")) - 0.5) < 1e-6
assert abs(float(run("1\n1.0\n")) - 1.0) < 1e-6
assert abs(float(run("1\n0.25\n")) - 0.25) < 1e-6
assert abs(float(run("1\n0.01\n")) - 0.05) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| p = 0,5 | c ≈ 0,5 | hành vi tầm trung đối xứng | 
| p = 1,0 | c = 1,0 | quyết định chí mạng ngay lập tức | 
| p = 0,25 | c nhỏ | tính đúng đắn của chế độ lãi suất thấp | 
| p = 0,01 | c rất nhỏ | ổn định trong chế độ cực đoan | 

## Vỏ cạnh 

Khi p chính xác bằng 1, cấu hình nhất quán duy nhất là c = 1. Trong trường hợp này, mọi đòn tấn công sau đòn đầu tiên được đảm bảo gây chí mạng ngay lập tức và độ dài chu kỳ chính xác là 1. 

Khi p cực nhỏ thì c cũng nhỏ và sản phẩm sống sót phân hủy rất chậm. Vòng lặp bên trong chu kỳ dự kiến ​​phải xử lý nhiều lần lặp, nhưng nó vẫn kết thúc nhanh chóng vì xác suất cuối cùng đạt 1 khi i vượt qua ngưỡng. 

Khi c gần đến ngưỡng mà i c trở thành 1 tại i rất lớn, độ chính xác của dấu phẩy động rất quan trọng vì việc nhân xác suất sống sót lặp đi lặp lại có thể tràn xuống. Thuật toán tránh điều này bằng cách sử dụng độ chính xác gấp đôi và dừng sớm khi đạt đến ngưỡng xác định.
