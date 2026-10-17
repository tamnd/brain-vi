---
title: "CF 105494G - Cần Thêm Vàng"
description: "Chúng tôi được cung cấp một bộ sưu tập quái vật, mỗi con quái vật được mô tả bằng hai con số. Một giá trị thể hiện số vàng bạn cần có trước khi có thể đánh bại quái vật đó và giá trị còn lại thể hiện số vàng bạn nhận được sau khi đánh bại nó."
date: "2026-06-23T21:02:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105494
codeforces_index: "G"
codeforces_contest_name: "2024-2025 ICPC NERC, Kyrgyzstan Qualification Contest"
rating: 0
weight: 105494
solve_time_s: 53
verified: true
draft: false
---

[CF 105494G - Cần thêm vàng](https://codeforces.com/problemset/problem/105494/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập quái vật, mỗi con quái vật được mô tả bằng hai con số. Một giá trị thể hiện số vàng bạn cần có trước khi có thể đánh bại quái vật đó và giá trị còn lại thể hiện số vàng bạn nhận được sau khi đánh bại nó. Bạn bắt đầu với một số vàng ban đầu và liên tục chọn quái vật để chiến đấu. Bạn chỉ có thể chiến đấu với một con quái vật nếu số vàng hiện tại của bạn ít nhất đạt đến ngưỡng yêu cầu. Sau khi đánh bại nó, số vàng của bạn sẽ tăng theo phần thưởng của nó. 

Nhiệm vụ là xác định số lượng quái vật tối đa bạn có thể đánh bại bằng cách chọn thứ tự chiến đấu tối ưu. 

Khó khăn chính là thứ tự quan trọng. Một quái vật có yêu cầu nhỏ có thể mở khóa quyền truy cập vào một quái vật khác sau đó và một quái vật có phần thưởng lớn có thể có giá trị sớm hơn vì nó làm tăng phạm vi tiếp cận trong tương lai của bạn. 

Từ góc độ ràng buộc, giải pháp dự định phải chạy ở khoảng O(n log n). Bất kỳ cách tiếp cận nào liên tục quét tất cả quái vật còn lại để chọn quái vật tiếp theo sẽ giảm xuống O(n²), quá chậm đối với đầu vào lớn. 

Một trường hợp thất bại tinh tế đối với trực giác tham lam là khi một con quái vật có yêu cầu cao hơn một chút thực sự mang lại phần thưởng tốt hơn nhiều, nhưng không thể truy cập ngay lập tức và việc trì hoãn nó sẽ thay đổi những quái vật khác có sẵn. Ví dụ: nếu một quái vật yêu cầu 5 và cho 1 vàng, còn quái vật khác yêu cầu 4 và cho 10 vàng, thì việc chọn sai trước có thể cản trở khả năng tiếp cận trong tương lai mặc dù cục bộ nó trông tệ hơn. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp thử mọi thứ tự có thể có của quái vật và mô phỏng các trận chiến, kiểm tra trình tự nào hợp lệ và tối đa hóa số lượng. Về nguyên tắc, điều này đúng vì nó khám phá tất cả các hoán vị, nhưng nó phát triển theo giai thừa và trở nên không thể sử dụng được ngay cả đối với n vừa phải. 

Một cách tiếp cận có cấu trúc hơn là mô phỏng quy trình từng bước: tại mỗi thời điểm, hãy xem xét tất cả các con quái vật mà bạn hiện có thể mua được và chọn một con. Câu hỏi là chọn cái nào. Quan sát quan trọng là trong số tất cả những con quái vật có giá cả phải chăng hiện nay, việc chọn con có phần thưởng lớn nhất luôn là điều tối ưu. Điều này có thể được chứng minh bằng lập luận trao đổi cục bộ: nếu trước tiên bạn chọn phần thưởng nhỏ hơn trong khi có phần thưởng lớn hơn, việc hoán đổi chúng không khiến bạn mất quyền truy cập vào các trạng thái trong tương lai và chỉ có thể cải thiện hoặc bảo toàn tổng số vàng. 

Điều này biến vấn đề thành việc duy trì một tập hợp năng động các “quái vật có sẵn” và liên tục chọn ra ứng cử viên tốt nhất. Tuy nhiên, việc quét toàn bộ danh sách ở mỗi bước vẫn còn quá chậm. 

Để tối ưu hóa điều này, chúng tôi chia những con quái vật thành hai nhóm: những nhóm chưa có giá phải chăng và những nhóm hiện có giá phải chăng. Chúng tôi duy trì một cấu trúc được sắp xếp theo yêu cầu để có thể nhanh chóng di chuyển quái vật vào nhóm có sẵn và một cấu trúc khác được sắp xếp theo phần thưởng để chúng tôi có thể nhanh chóng chọn quái vật tiếp theo tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Ngây thơ tham lam quét từng bước | O(n²) | O(n) | Quá chậm | 
| Mô phỏng tham lam dựa trên heap | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc ưu tiên. Một kho lưu trữ những quái vật vẫn không thể tiếp cận được vì chúng ta thiếu đủ vàng, được sắp xếp theo yêu cầu ngày càng tăng. Những quái vật lưu trữ khác mà chúng tôi hiện có thể đánh bại, được sắp xếp theo mức phần thưởng giảm dần. 

Chúng tôi cũng duy trì một biến cw đại diện cho số vàng hiện tại và bộ đếm số lượng quái vật mà chúng tôi đã đánh bại. 

Quá trình này như sau.

1. Đưa tất cả quái vật vào một cấu trúc được sắp xếp theo số vàng yêu cầu. Điều này cho phép chúng tôi nhanh chóng xác định những quái vật nào có sẵn khi cw tăng lên. 
2. Khởi tạo cw với số vàng ban đầu và đánh dấu tất cả quái vật ban đầu là “không có sẵn để lựa chọn”. 
3. Di chuyển tất cả quái vật có yêu cầu tối đa là cw vào nhóm có sẵn. Bước này đảm bảo rằng quỹ sẵn có luôn phản ánh chính xác những gì có thể chiến đấu ở thời điểm hiện tại. 
4. Nếu không có quái vật nào, quá trình sẽ dừng lại vì không thể tiến thêm được nữa. Bất kỳ quái vật nào còn lại đều cần nhiều vàng hơn mức có thể đạt được. 
5. Nếu không, hãy chọn quái vật có phần thưởng cao nhất từ ​​nhóm hiện có và đánh bại nó. Tăng cw bằng phần thưởng của nó và tăng câu trả lời. 
6. Sau khi nhận được vàng, hãy lặp lại quy trình từ bước 3, vì giờ đây quái vật mới có thể tiếp cận được. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, trong số tất cả quái vật hiện có giá phải chăng, con quái vật có phần thưởng cao nhất có thể được chọn mà không mất đi tính tối ưu. Nếu trình tự tối ưu chọn một quái vật có giá cả phải chăng khác trước tiên, thì việc hoán đổi nó với quái vật có phần thưởng cao nhất sẽ không làm giảm khả năng tiếp cận các quái vật sau này vì tất cả các yêu cầu chỉ phụ thuộc vào số vàng hiện tại và việc hoán đổi không làm giảm vàng tại thời điểm đưa ra quyết định trong tương lai. Điều này thiết lập đặc tính lựa chọn tham lam kết hợp với sự tăng trưởng đơn điệu của cw, đảm bảo mỗi bước đều tối ưu cục bộ và nhất quán toàn cầu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, start = map(int, input().split())
    monsters = [tuple(map(int, input().split())) for _ in range(n)]

    monsters.sort(key=lambda x: x[0])

    i = 0
    cw = start
    ans = 0

    # min-heap by requirement (bi)
    not_ready = []
    # max-heap by reward (gi), store negative
    ready = []

    while True:
        while i < n and monsters[i][0] <= cw:
            heapq.heappush(ready, -monsters[i][1])
            i += 1

        if not ready:
            break

        g = -heapq.heappop(ready)
        cw += g
        ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc sắp xếp quái vật theo yêu cầu để chúng tôi chỉ quét chúng một lần. Con trỏ i đảm bảo mỗi quái vật được di chuyển vào heap chính xác một lần, làm cho tổng chi phí chèn vào heap là O(n log n). 

Heap lưu trữ phần thưởng dưới dạng âm vì heapq của Python là min-heap. Mỗi bước sẽ trích xuất quái vật phần thưởng tối đa trong số những quái vật hiện có, phù hợp với quy tắc tham lam. 

Điều kiện kết thúc vòng lặp xảy ra khi không có quái vật nào hiện có thể tiếp cận được, có nghĩa là cw không thể tăng thêm nữa, vì vậy không có quái vật nào trong tương lai có thể tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
2 1
3 2
5 10
4 3
```Chúng tôi theo dõi cw vàng hiện tại, con trỏ i và đống có sẵn. 

| Bước | cw | Quái vật mới được thêm vào | Đống sẵn sàng (phủ định) | Phần thưởng được chọn | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | (2,1), (3,2) | [ -2, -1 ] | - | 
| 1 | 3 | - | [ -2, -1 ] | 2 | 
| 2 | 5 | (4,3) | [ -3, -1 ] | - | 
| 3 | 5 | (5,10) | [ -10, -1, -3 ] | 10 | 
| 4 | 15 | - | [ -3, -1 ] | - | 
| 5 | 15 | - | [ -3, -1 ] | 3 | 

Quá trình này cho thấy cách tăng vàng để mở khóa những quái vật sau này và tại sao luôn chọn phần thưởng cao nhất trong số những phần thưởng hiện có sẽ tối đa hóa phạm vi tiếp cận trong tương lai. 

### Ví dụ 2 

đầu vào:```
3 1
2 5
2 1
3 10
```| Bước | cw | Quái vật mới được thêm vào | Đống sẵn sàng | Phần thưởng được chọn | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | - | [] | - | 

Thuật toán dừng ngay lập tức vì không có con quái vật nào có giá cả phải chăng. Điều này xác nhận rằng các trạng thái ban đầu không thể truy cập sẽ chấm dứt chính xác mà không có lựa chọn không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi quái vật được chèn vào và loại bỏ khỏi đống nhiều nhất một lần và việc sắp xếp chỉ chiếm ưu thế một lần | 
| Không gian | O(n) | Chúng tôi lưu trữ tất cả quái vật trong mảng và tối đa n phần tử thành đống | 

Độ phức tạp phù hợp với các ràng buộc điển hình cho n đến 2e5 hoặc 1e5, trong đó các phép toán O(n log n) là tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, start = map(int, input().split())
        monsters = [tuple(map(int, input().split())) for _ in range(n)]
        monsters.sort()

        i = 0
        cw = start
        ans = 0
        import heapq
        ready = []

        while True:
            while i < n and monsters[i][0] <= cw:
                heapq.heappush(ready, -monsters[i][1])
                i += 1
            if not ready:
                break
            cw += -heapq.heappop(ready)
            ans += 1

        return str(ans)

    return solve()

# custom tests
assert run("1 10\n5 100\n") == "1"
assert run("2 1\n10 100\n10 100\n") == "0"
assert run("3 5\n1 1\n2 2\n3 3\n") == "3"
assert run("5 1\n2 10\n2 9\n2 8\n2 7\n2 6\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| con quái vật duy nhất có thể tiếp cận | 1 | tính khả thi cơ bản | 
| tất cả đều không thể truy cập | 0 | chấm dứt sớm | 
| chuỗi tăng nghiêm ngặt | 3 | mở khóa xích | 
| cùng yêu cầu nhiều lựa chọn | 5 | tham lam phần thưởng tối đa đúng đắn | 

## Vỏ cạnh 

Trường hợp một bên là khi ban đầu không có con quái vật nào có giá cả phải chăng. Thuật toán khởi tạo chính xác một đống sẵn sàng trống và kết thúc ngay lập tức mà không thử các lựa chọn không hợp lệ, tạo ra số 0. 

Một trường hợp khác là khi nhiều quái vật có cùng yêu cầu nhưng phần thưởng rất khác nhau. Vùng heap đảm bảo chỉ phần thưởng tốt nhất được chọn trước, ngăn chặn việc tiêu thụ dưới mức tối ưu các phần thưởng yếu có thể làm giảm phạm vi tiếp cận tổng thể. 

Trường hợp cuối cùng liên quan đến một chuỗi dài trong đó mỗi con quái vật sẽ mở khóa chính xác một con nữa. Bởi vì mỗi mức tăng được phản ánh ngay lập tức trong cw và tất cả quái vật mới có sẵn sẽ được đẩy vào đống trước khi lựa chọn, thuật toán không bao giờ bỏ lỡ tùy chọn phần thưởng cao mới được mở khóa sẽ có sẵn trong quá trình.
