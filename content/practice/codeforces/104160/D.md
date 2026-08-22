---
title: "CF 104160D - DRX so với T1"
description: "Chúng ta được cung cấp một chuỗi có độ dài cố định gồm 5 ký tự mô tả kết quả của chuỗi 5 ký tự hay nhất giữa DRX và T1."
date: "2026-07-02T01:03:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104160
codeforces_index: "D"
codeforces_contest_name: "The 2022 ICPC Asia Shenyang Regional Contest (The 1st Universal Cup, Stage 1: Shenyang)"
rating: 0
weight: 104160
solve_time_s: 51
verified: true
draft: false
---

[CF 104160D - DRX so với T1](https://codeforces.com/problemset/problem/104160/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi có độ dài cố định gồm 5 ký tự mô tả kết quả của chuỗi 5 ký tự hay nhất giữa DRX và T1. Mỗi nhân vật tương ứng với một trò chơi: DRX thắng, T1 thắng hoặc một biểu tượng đặc biệt cho biết chuỗi trận đã kết thúc trước đó và các trò chơi còn lại chưa bao giờ được chơi. 

Nguyên tắc quan trọng là đội đầu tiên đạt được 3 trận thắng sẽ trở thành nhà vô địch ngay lập tức và tất cả các trận đấu sau đó được thay thế bằng`?`biểu tượng. Điều này có nghĩa là chuỗi không chỉ là danh sách kết quả mà còn là bản ghi rút gọn của một cuộc đua tới 3 chiến thắng. 

Nhiệm vụ là xác định đội nào thực sự đạt được 3 trận thắng đầu tiên theo quy trình này. 

Mặc dù chuỗi có độ dài 5 nhưng điều quan trọng cần lưu ý là chỉ có tiền tố của nó mới chứa kết quả thực. Mọi thứ sau lần đầu tiên`?`không liên quan vì nó đại diện cho các trò chơi chưa được chơi. Vì vậy, thông tin hiệu quả là tiền tố nơi chúng tôi đếm số lần thắng cho đến khi đạt được điều kiện dừng. 

Các ràng buộc cực kỳ nhỏ, độ dài cố định 5, vì vậy bất kỳ giải pháp nào quét chuỗi một lần là đủ. Không cần cấu trúc dữ liệu nâng cao hoặc tổ hợp. Bất kỳ nỗ lực nào nhằm mô phỏng các nhánh trò chơi không cần thiết đều là quá mức cần thiết và sẽ chỉ làm phức tạp thêm khả năng suy luận đúng đắn. 

Ví dụ, một trường hợp phức tạp là khi bộ truyện kết thúc sau đúng 3 trò chơi`DDD??`hoặc`TTT??`. Một trường hợp khác là khi người chiến thắng đạt đến vị trí thứ 3 trước vị trí cuối cùng, chẳng hạn như`TDTT?`, trong đó ván thứ tư xác định người chiến thắng và ván thứ năm không liên quan. 

Một sai lầm ngây thơ là đếm tất cả các ký tự kể cả những ký tự đứng sau`?`, điều này sẽ hiểu không chính xác phần giữ chỗ là trò chơi thực. Một sai lầm khác là cho rằng luôn chơi cả 5 trò chơi, điều này vi phạm quy tắc cắt ngắn. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là coi tất cả 5 vị trí là trò chơi thực sự và mô phỏng số trận thắng trên toàn bộ chuỗi. Điều này có vẻ đơn giản nhưng lại thất bại về mặt khái niệm vì hậu tố sau ký tự đầu tiên`?`không đại diện cho các trận đấu thực tế. Nếu chúng ta cố gắng kết hợp nó, chúng ta có nguy cơ đếm được những trò chơi không tồn tại. 

Sự đơn giản hóa đúng đắn đến từ việc nhận ra rằng`?`là một điểm đánh dấu cắt cứng. Thời điểm nó xuất hiện, kết quả đã được quyết định, mọi xử lý tiếp theo đều vô nghĩa. Do đó, thay vì mô phỏng cây khớp đầy đủ hoặc xem xét việc hoàn thành giả định, chúng tôi chỉ xử lý tiền tố cho đến khi kết thúc. 

Điều này giúp giảm vấn đề xuống còn một lần quét tuyến tính với hai bộ đếm theo dõi chiến thắng DRX và T1. Chúng ta dừng lại sớm khi gặp phải`?`, và ngay lập tức quyết định xem ai đã đạt được 3 trận thắng tại thời điểm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force của cả 5 trò chơi | O(1) | O(1) | Không cần thiết nhưng hoạt động | 
| Tiền tố đếm cho đến khi`?`| O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải, duy trì bộ đếm chiến thắng cho cả hai đội. 

### bước 

1. Khởi tạo hai bộ đếm, một cho chiến thắng DRX và một cho chiến thắng T1, cả hai đều bắt đầu từ 0. Chúng đại diện cho các trò chơi đã chơi thực tế. 
2. Lặp lại chuỗi từ ký tự đầu tiên đến ký tự cuối cùng. Mỗi ký tự đại diện cho một kết quả trò chơi trừ khi đó là biểu tượng kết thúc. 
3. Nếu ký tự hiện tại là`D`, tăng bộ đếm của DRX. Nếu nó là`T`, tăng bộ đếm của T1. Điều này trực tiếp mô hình hóa diễn biến trận đấu. 
4. Sau khi xử lý từng trò chơi thực, hãy kiểm tra xem một trong hai đội có đạt được 3 trận thắng hay không. Nếu vậy, người chiến thắng sẽ được xác định ngay lập tức và chúng tôi có thể ngừng xử lý các ký tự tiếp theo. 
5. Nếu ký tự hiện tại là`?`, dừng ngay lập tức mà không xử lý thêm các vị trí vì không còn trò chơi nào được chơi nữa. 
6. In tên đội tương ứng với đội có 3 trận thắng đầu tiên. 

Sự lựa chọn thiết kế quan trọng đang dừng lại ở`?`thay vì tiếp tục suốt toàn bộ thời lượng, vì các vị trí sau này không tương ứng với các trận đấu thực tế. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố nào trước`?`, các bộ đếm thể hiện chính xác trạng thái khớp thực. Thời điểm một đội giành được 3 chiến thắng, chuỗi trò chơi sẽ kết thúc theo định nghĩa và phần còn lại của chuỗi là phần đệm cú pháp. Bởi vì đầu vào đảm bảo tính hợp lệ nên không có trường hợp nào`?`xuất hiện trước khi người chiến thắng tồn tại, vì vậy hãy dừng lại ở`?`hoặc dừng lại ở 3 trận thắng là điều kiện chấm dứt hợp đồng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    
    drx = 0
    t1 = 0
    
    for c in s:
        if c == '?':
            break
        if c == 'D':
            drx += 1
        else:
            t1 += 1
        
        if drx == 3:
            print("DRX")
            return
        if t1 == 3:
            print("T1")
            return

solve()
```Việc thực hiện là mô phỏng trực tiếp quá trình đối sánh. Vòng lặp dừng sớm khi chuỗi được quyết định hoặc khi phần giữ chỗ`?`xuất hiện. Việc quay lại sớm đảm bảo chúng tôi không vô tình tiếp tục quét các ký tự hậu tố không liên quan. 

Một chi tiết tinh tế là chúng tôi kiểm tra việc đạt được 3 chiến thắng ngay sau mỗi lần cập nhật. Điều này tránh việc lặp lại nhiều lần và phản ánh quy tắc trong thế giới thực rằng trận đấu kết thúc ngay lập tức khi một đội đạt được 3 chiến thắng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`TDTT?`Chúng tôi theo dõi chiến thắng từng bước cho đến khi chấm dứt. 

| Bước | Nhân vật | DRX thắng | T1 Thắng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | T | 0 | 1 | T1 thắng trận | 
| 2 | D | 1 | 1 | DRX thắng trò chơi | 
| 3 | T | 1 | 2 | T1 thắng trận | 
| 4 | T | 1 | 3 | T1 đạt 3 trận thắng, dừng lại | 

Quá trình dừng lại trước khi đọc`?`bởi vì người chiến thắng đã được xác định. Đầu ra là`T1`. 

### Ví dụ 2:`DTDDD`| Bước | Nhân vật | DRX thắng | T1 Thắng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | D | 1 | 0 | DRX thắng | 
| 2 | T | 1 | 1 | T1 thắng | 
| 3 | D | 2 | 1 | DRX thắng | 
| 4 | D | 3 | 1 | DRX đạt 3 trận thắng, dừng lại | 

Mặc dù không có`?`, quy tắc dừng tương tự được áp dụng khi một đội đạt được 3 trận thắng. Đầu ra là`DRX`. 

Những dấu vết này xác nhận rằng việc chấm dứt chỉ phụ thuộc vào việc đạt được 3 trận thắng chứ không phụ thuộc vào việc tiêu thụ toàn bộ chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Độ dài chuỗi được cố định ở mức 5, do đó vòng lặp chạy tối đa 5 lần lặp | 
| Không gian | O(1) | Chỉ có hai bộ đếm số nguyên được sử dụng | 

Thời gian chạy không đổi bất kể các biến thể đầu vào, dễ dàng phù hợp với mọi giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    
    def solve():
        s = sys.stdin.readline().strip()
        drx = 0
        t1 = 0
        for c in s:
            if c == '?':
                break
            if c == 'D':
                drx += 1
            else:
                t1 += 1
            if drx == 3:
                print("DRX")
                return
            if t1 == 3:
                print("T1")
                return
    
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("TDTT?\n") == "T1", "sample 1"
assert run("DTDD?\n") == "DRX", "sample 2"

# custom cases
assert run("DDD??\n") == "DRX", "instant sweep by DRX"
assert run("TTT??\n") == "T1", "instant sweep by T1"
assert run("DTDTD\n") == "DRX", "no '?' but early finish"
assert run("TDTDT\n") == "T1", "symmetric race"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| DDD?? | DRX | Chấm dứt sớm ở 3 trận thắng liên tiếp | 
| TT?? | T1 | Chấm dứt sớm đối xứng | 
| DTĐTD | DRX | KHÔNG`?`, người chiến thắng được quyết định bởi trận đấu cuối cùng | 
| TDTĐT | T1 | Thắng luân phiên, bước quyết định cuối cùng | 

## Vỏ cạnh 

Một trường hợp lợi thế quan trọng là khi trận đấu kết thúc càng sớm càng tốt, chẳng hạn như`DDD??`. Trong trường hợp này, thuật toán dừng chính xác ở ký tự thứ ba và các ký tự còn lại không bao giờ được xử lý. Bộ đếm đạt DRX = 3 ngay lập tức, do đó đầu ra là`DRX`. 

Một trường hợp khác là khi người chiến thắng được quyết định mà không có bất kỳ`?`xuất hiện, chẳng hạn như`DTDDD`. Thuật toán không dựa vào sự có mặt của`?`, chỉ khi đạt được 3 trận thắng. Quá trình quét tiếp tục cho đến khi gặp chiến thắng DRX thứ ba, sau đó kết thúc chính xác. 

Trường hợp cuối cùng là xen kẽ thắng như`TDTDT`, nơi không đội nào chiếm ưu thế sớm. Thuật toán đảm bảo tính chính xác bằng cách kiểm tra sau mỗi lần tăng nên khi T1 đạt 3 chiến thắng, nó sẽ dừng ngay cả khi có ký tự sau đó.
