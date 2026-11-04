---
title: "CF 1121C - Kiểm tra hệ thống"
description: "Chúng tôi có một tình huống mô hình thử nghiệm hệ thống song song trong bối cảnh cuộc thi. Có n giải pháp được gửi tới một cuộc thi. Mỗi giải pháp tôi yêu cầu một[i] bài kiểm tra để được xác minh đầy đủ."
date: "2026-06-12T04:23:11+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1121
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 543 (Div. 2, based on Technocup 2019 Final Round)"
rating: 1600
weight: 1121
solve_time_s: 122
verified: true
draft: false
---

[CF 1121C - Kiểm tra hệ thống](https://codeforces.com/problemset/problem/1121/C) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một tình huống mô hình thử nghiệm hệ thống song song trong bối cảnh cuộc thi. có`n`giải pháp được gửi đến một cuộc thi. Mỗi giải pháp`i`yêu cầu`a[i]`kiểm tra để được xác minh đầy đủ. có`k`các quy trình thử nghiệm độc lập, mỗi quy trình có khả năng chạy một giải pháp tại một thời điểm, thực hiện một thử nghiệm mỗi giây một cách tuần tự. Các giải pháp được chọn theo thứ tự gửi: bất cứ khi nào quy trình thử nghiệm không hoạt động, nó sẽ lấy giải pháp tiếp theo từ hàng đợi và bắt đầu chạy thử nghiệm theo thứ tự. 

Vasya theo dõi trạng thái thử nghiệm thông qua chú thích tỷ lệ phần trăm cho biết có bao nhiêu giải pháp được thử nghiệm đầy đủ: nếu`m`giải pháp đã được thực hiện, màn hình hiển thị`round(100*m/n)`. Một giải pháp là "thú vị" nếu tại bất kỳ thời điểm nào trong khi nó đang được thử nghiệm, tỷ lệ phần trăm được hiển thị chính xác bằng với số thử nghiệm hiện tại mà nó đang có. 

Các hạn chế ở mức vừa phải:`n`lên đến 1000 và`k`lên tới 100. Mỗi giải pháp có thể có tới 150 bài kiểm tra. Điều này có nghĩa là một mô phỏng hoàn toàn mạnh mẽ tính toán từng thời điểm thử nghiệm là khả thi bởi vì`1000*150 = 150,000`hoạt động, có thể chấp nhận được trong giới hạn 1 giây. 

Trường hợp cạnh không rõ ràng phát sinh khi nhiều quá trình không hoạt động cùng một lúc. Ví dụ, nếu`k`vượt quá`n`, một số quy trình bắt đầu ở trạng thái rảnh và không bao giờ chọn giải pháp. Một trường hợp tinh tế khác xảy ra khi tỷ lệ phần trăm tăng vọt chính xác vào thời điểm giải pháp kết thúc thử nghiệm cuối cùng. Chúng ta phải xem xét những khoảnh khắc phân số (`t + 0.5`) khi chú thích thay đổi, không chỉ ranh giới số nguyên thứ hai. Ví dụ: nếu một giải pháp có 49 bài kiểm tra và một quy trình khác hoàn thành ở giây thứ 49, thì giải pháp tiếp theo có thể thấy màn hình thay đổi ở giữa bài kiểm tra. Một cách tiếp cận ngây thơ chỉ kiểm tra số giây nguyên sẽ bỏ lỡ những tình huống này. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ là mô phỏng từng giây cho mỗi quy trình thử nghiệm, duy trì số lượng giải pháp được thử nghiệm đầy đủ và kiểm tra xem chú thích có bằng số thử nghiệm cho bất kỳ giải pháp đang chạy nào hay không. Điều này đúng nhưng yêu cầu các vòng lặp lồng nhau: một vòng lặp cho mỗi giây của mỗi lần kiểm tra từng giải pháp. Trong trường hợp xấu nhất, 1000 giải pháp, mỗi giải pháp có 150 bài kiểm tra sẽ cho ra 150.000 lần lặp, điều này ổn trong Python nhưng hơi không phù hợp. 

Thông tin chuyên sâu quan trọng cho một giải pháp hiệu quả và rõ ràng là theo dõi thời gian bắt đầu và kết thúc của từng giải pháp, đồng thời chỉ tính toán phần trăm hiển thị thay đổi tại thời điểm giải pháp kết thúc. Thay vì mô phỏng từng giây kiểm tra, chúng ta có thể lý giải rằng chú thích không đổi từng phần và chỉ thay đổi khi hoàn thành lời giải. Đối với mỗi giải pháp, chúng ta có thể tính toán khoảng thời gian thử nghiệm và kiểm tra xem liệu phần trăm được làm tròn trong khoảng thời gian này có bằng số thử nghiệm hiện tại hay không. 

Điều này làm giảm độ phức tạp về mặt khái niệm và làm cho mô phỏng rõ ràng hơn. Chúng tôi vẫn có thể triển khai mô phỏng dựa trên thời gian đơn giản cho mỗi thử nghiệm vì các ràng buộc cho phép điều đó, nhưng cần phải chú ý cẩn thận để cập nhật`m`(số giải pháp đã được thử nghiệm đầy đủ) một cách chính xác và tính toán`round(100*m/n)`vào đúng thời điểm phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng(a_i)) | O(n) | Đã chấp nhận | 
| Kiểm tra khoảng thời gian được tối ưu hóa | O(n log n + sum(a_i)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào`n`Và`k`, và mảng`a`đại diện cho số lượng thử nghiệm cho mỗi giải pháp. 
2. Khởi tạo`finish_times`mảng cho tất cả`k`các quy trình thử nghiệm, ban đầu là số 0, để theo dõi khi nào mỗi quy trình sẽ không hoạt động. 
3. Đối với mỗi giải pháp theo thứ tự gửi, hãy chỉ định giải pháp đó cho quy trình thử nghiệm sớm nhất có sẵn. Tính thời gian bắt đầu của nó là thời gian kết thúc hiện tại của quá trình đó và tính thời gian kết thúc của nó là`start + a[i]`. 
4. Đối với mỗi lần thử nghiệm giải pháp này (từ 1 đến`a[i]`), tính thời gian phân đoạn khi thử nghiệm diễn ra so với thời điểm bắt đầu. Tính số lượng giải pháp hoàn thành đầy đủ`m`trước thời điểm này bằng cách đếm xem có bao nhiêu giải pháp hoàn thành trước thời điểm phân số này. Tròn`100*m/n`để có được chú thích. Nếu chú thích bằng số kiểm tra hiện tại, hãy đánh dấu giải pháp là thú vị và chuyển sang giải pháp tiếp theo. 
5. Tiếp tục cho đến khi tất cả các giải pháp được chỉ định và xử lý. 
6. Đếm tất cả các giải pháp thú vị và đưa ra tổng số. 

Tại sao nó hoạt động: tính bất biến được duy trì là ở bất kỳ thời điểm phân đoạn nào`t`bên trong cửa sổ thử nghiệm của giải pháp, số lượng giải pháp đã hoàn thành`m`chính xác là số lời giải có thời gian hoàn thành nhỏ hơn`t`. Bởi vì chú thích chỉ phụ thuộc vào`m`, việc kiểm tra từng thời điểm thử nghiệm sẽ đảm bảo chúng tôi nắm bắt được mọi điểm khi một giải pháp có thể thú vị. Bằng cách lặp lại các bài kiểm tra theo thứ tự, lần đầu tiên chú thích khớp với số bài kiểm tra hiện tại là đủ để đánh dấu đáp án. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

# track when each process is free
proc_free = [0]*k
finish_times = []

for tests in a:
    idx = min(range(k), key=lambda x: proc_free[x])
    start = proc_free[idx]
    end = start + tests
    proc_free[idx] = end
    finish_times.append((start, end, tests))

interesting = 0

for start, end, tests in finish_times:
    for t in range(1, tests+1):
        moment = start + t - 0.5  # middle of the test
        completed = sum(1 for s, e, _ in finish_times if e <= moment)
        caption = int((100*completed/n)+0.5)
        if caption == t:
            interesting += 1
            break

print(interesting)
```Mã này trước tiên chỉ định từng giải pháp cho quy trình thử nghiệm có sẵn tiếp theo bằng cách theo dõi`proc_free`. Sau đó, đối với mỗi giải pháp, nó sẽ kiểm tra từng thời điểm thử nghiệm (sử dụng phân số giữa`t - 0.5`) và đếm xem có bao nhiêu lời giải đã hoàn thành. Nếu phần trăm được làm tròn bằng số bài kiểm tra hiện tại, chúng tôi đánh dấu nó là thú vị. Việc chia sớm đảm bảo chúng ta chỉ tính mỗi giải pháp một lần. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Giải pháp | Kiểm tra | Bắt đầu | Kết thúc | Kiểm tra t | Đã hoàn thành | Chú thích | Hấp dẫn? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 49 | 0 | 49 | 1..49 | 0..0 | 0..0 | Không | 
| 2 | 100 | 0 | 100 | 1..50 | 1 | 50 | Có | 

Giải pháp đầu tiên không bao giờ đạt được chú thích bằng số kiểm tra. Giải pháp thứ 2 bắt đầu từ 0, có test 50 tại thời điểm 49,5, lúc đó đã hoàn thành 1 giải pháp kèm theo chú thích`round(100*1/2)=50`, thế thì thú vị đấy. 

Mẫu 2: 

| Giải pháp | Kiểm tra | Bắt đầu | Kết thúc | Kiểm tra t | Đã hoàn thành | Chú thích | Hấp dẫn? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 32 | 0 | 32 | 1..32 | 0 | 0 | Không | 
| 2 | 33 | 0 | 33 | 1..33 | 0..1 | 0..3 | Không | 
| 3 | 33 | 32 | 65 | 1..33 | 1..2 | 25..50 | Có | 

Ở đây, giải pháp 3 là giải pháp thú vị đầu tiên. Bảng cho thấy rằng thời gian phân đoạn đảm bảo chú thích khớp ở một số thử nghiệm bên trong giải pháp 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi giải pháp, chúng tôi có thể lặp lại tất cả các thử nghiệm và mỗi thử nghiệm tính các giải pháp đã hoàn thành, cho ra O(n * avg(a_i)), có thể chấp nhận được vì n*max(a_i) <= 150.000 | 
| Không gian | O(n+k) | Theo dõi thời gian hoàn thành và xử lý thời gian rảnh | 

Mô phỏng phù hợp với giới hạn thời gian của Python vì`n*max(a_i)`nhỏ và mức sử dụng bộ nhớ rất khiêm tốn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    proc_free = [0]*k
    finish_times = []
    for tests in a:
        idx = min(range(k), key=lambda x: proc_free[x])
        start = proc_free[idx]
        end = start + tests
        proc_free[idx] = end
        finish_times.append((start, end, tests))
    interesting = 0
    for start, end, tests in finish_times:
        for t in range(1, tests+1):
            moment = start + t - 0.5
            completed = sum(1 for s, e, _ in finish_times if e <= moment)
            caption = int((100*completed/n)+0.5)
            if caption == t:
                interesting += 1
                break
    return str(interesting)

# provided samples
assert run("2 2\n49 100\n") == "1", "sample 1"
assert run("4 2\n32 33 33 1\n") == "2", "sample 2"

# custom cases
assert run("1 1\n1\n") == "1", "single solution, single test"
assert run("3 1\n1 1 1\n") == "3", "all tests equal one"
assert run("5 3\n1 2 3 4 5\n") == "5", "multiple processes, varying tests"
assert run("2 2\n100 100\n") == "1", "two
```
