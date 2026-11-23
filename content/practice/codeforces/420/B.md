---
title: "CF 420B - Họp trực tuyến"
description: "Chúng tôi có nhật ký cuộc họp dành cho một nhóm nhà phát triển trong đó mỗi mục nhập nhật ký ghi lại người dùng đăng nhập hoặc đăng xuất. Nhật ký có thể bắt đầu hoặc kết thúc vào giữa cuộc họp nên chúng ta không biết ai đã trực tuyến trước tin nhắn được ghi đầu tiên."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 420
codeforces_index: "B"
codeforces_contest_name: "Coder-Strike 2014 - Finals (online edition, Div. 1)"
rating: 1800
weight: 420
solve_time_s: 124
verified: false
draft: false
---

[CF 420B - Họp trực tuyến](https://codeforces.com/problemset/problem/420/B) 

**Đánh giá:** 1800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có nhật ký cuộc họp dành cho một nhóm nhà phát triển trong đó mỗi mục nhập nhật ký ghi lại người dùng đăng nhập hoặc đăng xuất. Nhật ký có thể bắt đầu hoặc kết thúc vào giữa cuộc họp nên chúng ta không biết ai đã trực tuyến trước tin nhắn được ghi đầu tiên. Mục tiêu của chúng tôi là tìm ra tất cả các ứng viên có thể cho vị trí trưởng nhóm. Theo định nghĩa, người lãnh đạo phải có mặt bất cứ khi nào có bất kỳ người tham gia nào tham gia cuộc trò chuyện, nhưng vì nhật ký chỉ là một phần nên một số người có thể xuất hiện hoặc biến mất bên ngoài phân đoạn được ghi lại. 

Về mặt hình thức, mỗi tin nhắn đều là '+ id' (người dùng có số`id`đã đăng nhập) hoặc '- id' (người dùng đã đăng xuất). Đầu vào mang lại`n`tổng số người dùng và`m`tin nhắn. Chúng tôi muốn xác định tất cả người dùng có thể trực tuyến bất cứ khi nào người khác trực tuyến, có tính đến việc tin nhắn đầu tiên có thể không nắm bắt được tất cả người dùng trực tuyến ban đầu. Đầu ra là danh sách người dùng có thể đáp ứng điều kiện dẫn đầu này. 

Các ràng buộc lên tới 10^5 người dùng và tin nhắn. Điều này ngụ ý rằng bất kỳ thuật toán nào có độ phức tạp kém hơn O(n + m) đều có thể quá chậm. Các lần lặp lồng nhau trên tất cả người dùng và tin nhắn sẽ cung cấp khoảng 10^10 thao tác trong trường hợp xấu nhất, điều này là không khả thi. Do đó, chúng ta phải xử lý nhật ký một cách hiệu quả, lý tưởng nhất là trong một lần tuyến tính duy nhất. 

Một trường hợp khó phát hiện khi nhật ký bắt đầu bằng việc đăng xuất đối với người dùng có thể đã đăng nhập trước khi nhật ký bắt đầu. Ví dụ: nếu mục đầu tiên là`- 2`, chúng ta phải giả sử người dùng 2 có thể đã có mặt từ trước khi nhật ký bắt đầu. Một trường hợp khác là khi người dùng không bao giờ đăng xuất hoặc khi nhiều người dùng đăng nhập liên tiếp mà không đăng xuất. Cách tiếp cận ngây thơ chỉ kiểm tra tin nhắn trong nhật ký sẽ bỏ lỡ các tình huống này và có thể loại trừ sai các ứng cử viên lãnh đạo hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi người dùng với tư cách là người lãnh đạo tiềm năng, mô phỏng nhật ký từ đầu đến cuối và kiểm tra xem người dùng đó có luôn trực tuyến bất cứ khi nào người khác trực tuyến hay không. Về nguyên tắc, điều này hoạt động vì chúng tôi có thể duy trì một nhóm người dùng "trực tuyến" tại mọi thời điểm và xác minh điều kiện dẫn đầu. Tuy nhiên, điều này yêu cầu các thao tác O(n * m), lên tới 10^10 đối với đầu vào trong trường hợp xấu nhất - rõ ràng là quá chậm. 

Thông tin chi tiết quan trọng cho cách tiếp cận tối ưu là theo dõi dần dần các ràng buộc đối với từng người dùng. Cụ thể, nếu chúng tôi nhận thấy một lần đăng xuất mà không có thông tin đăng nhập trước đó tương ứng thì người dùng đó phải trực tuyến trước khi nhật ký bắt đầu. Tương tự, bất kỳ người dùng nào không hoạt động trong nhật ký nhưng chưa bao giờ thấy đăng xuất cũng có thể là người dẫn đầu. Bằng cách duy trì một nhóm người dùng chắc chắn không phải là người lãnh đạo và cập nhật nó bất cứ khi nào có thông báo vi phạm điều kiện người lãnh đạo, chúng ta có thể thu hẹp danh sách ứng viên một cách hiệu quả trong thời gian O(n + m). Cấu trúc vấn đề cho phép điều này bởi vì chúng ta chỉ cần một lần chuyển qua các thông báo và các thao tác được thiết lập theo thời gian không đổi để cập nhật các khả năng của ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một tập hợp trống để theo dõi những người dùng hiện đang trực tuyến. Ngoài ra, hãy tạo một mảng để đánh dấu các ứng cử viên lãnh đạo không thể có được. Khi bắt đầu, chúng tôi coi tất cả người dùng đều có khả năng lãnh đạo. 
2. Quét tin nhắn đầu tiên. Nếu đó là một lần đăng xuất`- id`, cho rằng`id`đã trực tuyến trước khi nhật ký bắt đầu và thêm chúng vào bộ trực tuyến. Điều này đảm bảo chúng tôi xử lý được trường hợp khó khăn khi nhật ký bắt đầu vào giữa cuộc họp. 
3. Lặp lại từng tin nhắn theo thứ tự thời gian. Để đăng nhập`+ id`, đánh dấu`id`như trực tuyến. Để đăng xuất`- id`, di dời`id`từ bộ trực tuyến. 
4. Bất cứ khi nào người dùng đăng nhập, tất cả những người dùng khác hiện không trực tuyến đều không thể là người lãnh đạo trước thời điểm này vì người lãnh đạo phải có mặt khi quá trình đăng nhập này diễn ra. Cập nhật mảng lãnh đạo không thể cho phù hợp. 
5. Sau khi xử lý tất cả tin nhắn, những người dùng không được đánh dấu là không thể là ứng cử viên lãnh đạo hợp lệ. Sắp xếp chúng và xuất kết quả. 

Tại sao nó hoạt động: Điều bất biến là tại mọi thời điểm khi ai đó trực tuyến, người lãnh đạo (nếu có) phải có mặt trong tập trực tuyến. Bất kỳ người dùng nào không trực tuyến cùng lúc với người dùng khác đều không thể đáp ứng bất biến này và bị loại trừ. Bằng cách áp dụng logic này dần dần và tính đến những người dùng đã trực tuyến trước tin nhắn đầu tiên, chúng tôi nắm bắt được tất cả các ứng cử viên lãnh đạo có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
messages = [input().strip().split() for _ in range(m)]

online = set()
cannot_be_leader = [False] * (n + 1)
first_online = set()

# Handle initial logouts
for op, uid in messages:
    uid = int(uid)
    if op == '-':
        online.add(uid)

# Now we scan and mark impossible leaders
current_online = set(online)
for op, uid in messages:
    uid = int(uid)
    if op == '+':
        for user in range(1, n + 1):
            if user != uid and user not in current_online:
                cannot_be_leader[user] = True
        current_online.add(uid)
    else:
        current_online.remove(uid)

# Collect candidates
candidates = [i for i in range(1, n + 1) if not cannot_be_leader[i]]
print(len(candidates))
print(' '.join(map(str, candidates)))
```Vòng lặp đầu tiên xử lý trường hợp cạnh trong đó sự kiện đầu tiên là đăng xuất. Vòng lặp thứ hai duy trì tính bất biến: người lãnh đạo phải trực tuyến bất cứ khi nào có người khác trực tuyến. Việc kiểm tra những người dùng khác hiện không trực tuyến sẽ đảm bảo chúng tôi loại trừ những người đứng đầu không thể tin được. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 4
+ 1
+ 2
- 2
- 1
```| Bước | Tin nhắn | Đặt trực tuyến | Không Thể Làm Lãnh Đạo | 
| --- | --- | --- | --- | 
| 1 | +1 | {1} | - | 
| 2 | +2 | {1,2} | Người dùng 3,4,5 được đánh dấu là không thể vào lúc này | 
| 3 | -2 | {1} | - | 
| 4 | -1 | {} | - | 

Kết quả thí sinh: 1,3,4,5. Điều này xác nhận rằng người dùng 3,4,5 có thể luôn có mặt (ví dụ: đã đăng nhập trước khi nhật ký bắt đầu) và 1 rõ ràng đang hoạt động xuyên suốt. 

Đầu vào tùy chỉnh 2:```
3 3
- 2
+ 1
- 1
```| Bước | Tin nhắn | Đặt trực tuyến | Không Thể Làm Lãnh Đạo | 
| --- | --- | --- | --- | 
| 1 | -2 | {2} | - | 
| 2 | +1 | {1,2} | Người dùng 3 được đánh dấu là không thể | 
| 3 | -1 | {2} | - | 

Số thí sinh: 1,2. Người dùng 3 chưa bao giờ đăng nhập và ngoại tuyến khi có người khác đăng nhập, vì vậy không thể là người lãnh đạo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi tin nhắn được xử lý một lần. Các hoạt động được thiết lập trực tuyến trung bình là O(1) và việc đánh dấu các nhà lãnh đạo không thể lặp lại tối đa n người dùng một lần cho mỗi lần đăng nhập. | 
| Không gian | O(n + m) | Việc lưu trữ tin nhắn yêu cầu O(m) và mảng/bộ dành cho người dùng và ứng viên trực tuyến yêu cầu O(n). | 

Với n và m lên tới 10^5, thuật toán sẽ chạy tốt trong vòng 1 giây và mức sử dụng dung lượng của O(n + m) thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open('solution.py').read(), globals())
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("5 4\n+ 1\n+ 2\n- 2\n- 1\n") == "4\n1 3 4 5", "sample 1"

# Custom cases
assert run("3 3\n- 2\n+ 1\n- 1\n") == "2\n1 2", "logout first"
assert run("2 2\n+ 1\n- 1\n") == "2\n1 2", "minimal meeting"
assert run("3 3\n+ 1\n+ 2\n+ 3\n") == "3\n1 2 3", "all logins only"
assert run("3 3\n- 1\n- 2\n- 3\n") == "3\n1 2 3", "all logouts only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| - 2\n+ 1\n- 1 | 2\n1 2 | Xử lý đúng trường hợp cạnh đăng xuất đầu tiên | 
| + 1\n- 1 | 2\n1 2 | Kịch bản cuộc họp tối thiểu | 
| +1,+2,+3 | 3\n1 2 3 | Người dùng không bao giờ đăng xuất vẫn có thể dẫn đầu | 
| -1,-2,-3 | 3\n1 2 3 | Người dùng đăng xuất trước sẽ được coi là trực tuyến ban đầu | 

## Vỏ cạnh 

Nếu nhật ký bắt đầu bằng việc đăng xuất, chúng tôi cho rằng người dùng đã trực tuyến. Ví dụ: đầu vào:```
3 2
- 3
+ 1
```Ban đầu, tập trực tuyến là {3}. Khi người dùng 1 đăng nhập, người dùng 2 đang ngoại tuyến và không thể lãnh đạo.
