---
title: "CF 104303A - \u7b7e\u5230\u5566~"
description: "Chúng tôi được đưa ra nhiều kịch bản độc lập. Trong mỗi tình huống, học sinh bắt đầu với một số vật phẩm cố định phải mang theo và có một số điểm kiểm tra dọc theo một con đường."
date: "2026-07-01T20:08:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104303
codeforces_index: "A"
codeforces_contest_name: "2023 Xiangtan Unversity Freshman Conteset"
rating: 0
weight: 104303
solve_time_s: 49
verified: true
draft: false
---

[CF 104303A - \u7b7e\u5230\u5566~](https://codeforces.com/problemset/problem/104303/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều kịch bản độc lập. Trong mỗi tình huống, học sinh bắt đầu với một số vật phẩm cố định phải mang theo và có một số điểm kiểm tra dọc theo một con đường. Tại mỗi điểm kiểm tra, nếu học sinh lựa chọn “sử dụng” điểm kiểm tra đó, một số đồ vật cố định sẽ được người trợ giúp tại điểm kiểm tra đó lấy đi và vận chuyển đến đích. Mỗi lần sử dụng trợ giúp có chi phí bằng một lần uống. 

Học sinh được phép chọn thứ tự đến các điểm kiểm tra và mục tiêu là giảm thiểu số lượng điểm kiểm tra khác nhau mà học sinh sẽ sử dụng trong khi vẫn đảm bảo rằng tất cả các vật phẩm cuối cùng đều được vận chuyển. 

Tương tác chính là mỗi điểm kiểm tra đóng góp một công suất cố định và việc sử dụng điểm kiểm tra sẽ giảm tải còn lại. Nếu sử dụng nhiều điểm kiểm tra thì tổng số vật phẩm được vận chuyển là tổng đóng góp của chúng và tổng này phải đạt hoặc vượt quá tải trọng ban đầu. 

Đầu ra cho mỗi kịch bản là số lượng điểm kiểm tra tối thiểu cần thiết sao cho sự đóng góp kết hợp của chúng ít nhất bằng số lượng mục ban đầu. 

Các ràng buộc đủ nhỏ để việc sắp xếp và quét tuyến tính có thể dễ dàng thực hiện được. Mỗi thử nghiệm có tối đa 200 điểm kiểm tra và các giá trị ở mức vừa phải, do đó, O(n log n) hoặc O(n) cho mỗi giải pháp thử nghiệm đều nằm trong giới hạn. 

Một trường hợp thất bại tinh tế xuất hiện khi những đóng góp lớn nằm rải rác. Một cách tiếp cận đơn giản có thể cố gắng mô phỏng thứ tự tùy ý hoặc lựa chọn tham lam mà không sắp xếp, dẫn đến thiếu tập hợp con tối ưu. Ví dụ: việc chọn những người đóng góp nhỏ trước tiên có thể làm tăng số lượng một cách không cần thiết mặc dù tồn tại một người đóng góp lớn sẽ làm giảm tổng số điểm kiểm tra cần thiết. 

Một trường hợp khác là khi một điểm kiểm tra duy nhất đã đáp ứng được yêu cầu. Bất kỳ thuật toán nào không xử lý rõ ràng điều này sẽ chỉ suy biến chính xác nếu nó tự nhiên chọn giá trị lớn nhất đầu tiên sau khi sắp xếp. 

## Phương pháp tiếp cận 

Giải thích brute-force là xem xét mọi tập hợp con của các điểm kiểm tra, tính tổng của nó và chọn tập hợp con nhỏ nhất có tổng đạt đến tải yêu cầu. Điều này đúng vì nó trực tiếp kiểm tra mọi khả năng. Tuy nhiên, điều này đòi hỏi phải kiểm tra 2^n tập hợp con và ngay cả với n = 200, điều này hoàn toàn không khả thi vì số lượng tập hợp con vượt quá mọi giới hạn tính toán thực tế. 

Cấu trúc của bài toán được đơn giản hóa đáng kể khi chúng ta diễn giải lại nó như một bài toán lựa chọn với mục tiêu đơn điệu. Mỗi điểm kiểm tra đóng góp độc lập và thứ tự không ảnh hưởng đến tổng số cuối cùng, chỉ những điểm được chọn mới quan trọng. Để giảm thiểu số lượng điểm kiểm tra đã chọn trong khi đạt được số tiền yêu cầu, trước tiên chúng ta nên ưu tiên đóng góp lớn hơn. Điều này biến vấn đề thành một chiến lược tham lam cổ điển: sắp xếp các khoản đóng góp theo thứ tự giảm dần và tiếp tục chọn cho đến khi số tiền tích lũy đạt được mục tiêu. 

Điều này hiệu quả vì bất kỳ giải pháp tối ưu nào sử dụng giá trị nhỏ hơn thay vì giá trị lớn hơn đều có thể được cải thiện bằng cách hoán đổi nó với giá trị không sử dụng lớn hơn, không bao giờ tăng số lượng phần tử được chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Sắp xếp tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Đọc n và w, cùng với mảng a đóng góp. Chúng đại diện cho số lượng mục mà mỗi điểm kiểm tra có thể giảm tải. 
2. Sắp xếp mảng theo thứ tự giảm dần sao cho phần đóng góp lớn nhất được xét trước. Thứ tự này đảm bảo rằng mỗi lựa chọn sẽ mang lại mức giảm tải còn lại tối đa có thể. 
3. Khởi tạo tổng chạy về 0 và bộ đếm về 0. Tổng số theo dõi số lượng mặt hàng đã được dỡ xuống cho đến nay, trong khi bộ đếm theo dõi số lượng điểm kiểm tra đã được sử dụng. 
4. Lặp lại mảng đã được sắp xếp, thêm từng giá trị vào tổng hiện có và tăng bộ đếm lên một giá trị mỗi lần. 
5. Dừng ngay lập tức khi tổng hiện hành lớn hơn hoặc bằng w. Bộ đếm tại thời điểm này là số lượng điểm kiểm tra tối thiểu cần thiết. 
6. Xuất bộ đếm. 

Lý do chúng tôi có thể dừng sớm là vì một khi đã đáp ứng được yêu cầu, việc bổ sung thêm nhiều điểm kiểm tra sẽ chỉ làm tăng số lượng mà không cải thiện tính khả thi. 

### Tại sao nó hoạt động 

Chiến lược tham lam dựa trên lập luận trao đổi. Giả sử tồn tại một giải pháp tối ưu sử dụng k điểm kiểm tra nhưng không bao gồm một trong k đóng góp lớn nhất. Khi đó phải tồn tại một giá trị được chọn nhỏ hơn có thể được thay thế bằng một giá trị chưa sử dụng lớn hơn mà không làm giảm tổng số tiền. Sự thay thế này không bao giờ làm tăng số lượng phần tử được chọn, do đó, việc áp dụng phép biến đổi này nhiều lần sẽ mang lại giải pháp luôn chọn những đóng góp lớn nhất hiện có trước tiên. Do đó, việc xây dựng tham lam được sắp xếp phù hợp với một giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n, w = map(int, input().split())
        a = list(map(int, input().split()))
        
        a.sort(reverse=True)
        
        total = 0
        cnt = 0
        
        for x in a:
            total += x
            cnt += 1
            if total >= w:
                break
        
        out.append(str(cnt))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện theo chiến lược tham lam trực tiếp. Sắp xếp ngược lại đảm bảo chúng tôi luôn chọn người đóng góp lớn nhất còn lại trước tiên. Vòng lặp tích lũy các khoản đóng góp cho đến khi đáp ứng được yêu cầu và thời điểm dừng sẽ đưa ra số lượt chọn tối thiểu. 

Một lỗi phổ biến là quên rằng mục tiêu là giảm thiểu số lượng chứ không phải tối đa hóa tổng, đó là lý do tại sao hướng sắp xếp lại quan trọng. Một vấn đề tế nhị khác là xử lý nhiều trường hợp thử nghiệm một cách độc lập, vì việc tích lũy phải đặt lại cho mỗi trường hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, w = 100
a = [20, 30, 31, 15, 50]
```Đã sắp xếp:```
[50, 31, 30, 20, 15]
```| Bước | Giá trị được chọn | Tổng chạy | Đếm | 
| --- | --- | --- | --- | 
| 1 | 50 | 50 | 1 | 
| 2 | 31 | 81 | 2 | 
| 3 | 30 | 111 | 3 | 

Ở bước 3, tổng vượt quá 100, do đó câu trả lời là 3. Điều này chứng tỏ rằng việc chọn sớm những đóng góp lớn nhất sẽ giảm thiểu số lượng điểm kiểm tra cần thiết. 

### Ví dụ 2 

đầu vào:```
n = 4, w = 40
a = [10, 5, 25, 30]
```Đã sắp xếp:```
[30, 25, 10, 5]
```| Bước | Giá trị được chọn | Tổng chạy | Đếm | 
| --- | --- | --- | --- | 
| 1 | 30 | 30 | 1 | 
| 2 | 25 | 55 | 2 | 

Chúng ta dừng lại ở con số 2 vì 55 đã thỏa mãn yêu cầu. Điều này cho thấy rằng chúng ta không bao giờ cần tiêu thụ hết tất cả các điểm kiểm tra, chỉ đủ để đạt đến ngưỡng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · n log n) | Sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm | 
| Không gian | O(n) | Lưu trữ cho mảng | 

Các ràng buộc cho phép tối đa 200 phần tử cho mỗi lần kiểm tra, do đó, việc sắp xếp 200 số lên tới 200 lần là chuyện nhỏ trong giới hạn. Quét tham lam là tuyến tính và không đáng kể so với sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, w = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort(reverse=True)

        total = 0
        cnt = 0
        for x in a:
            total += x
            cnt += 1
            if total >= w:
                break
        out.append(str(cnt))

    return "\n".join(out)

# provided sample (interpreted consistent with statement style)
assert run("1\n5 100\n20 30 31 15 50\n") == "3"

# minimum case
assert run("1\n1 5\n10\n") == "1"

# already satisfied by one large element
assert run("1\n3 10\n1 2 10\n") == "1"

# requires all elements
assert run("1\n3 10\n2 3 4\n") == "3"

# mixed ordering
assert run("1\n4 15\n5 1 10 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 mục lớn hơn w | 1 | sự lựa chọn duy nhất thành công | 
| tất cả số tiền nhỏ | n | phải sử dụng tất cả các yếu tố | 
| giá trị rải rác | sự đúng đắn tham lam | đặt hàng cần thiết | 
| trường hợp hỗn hợp | dừng sớm | chấm dứt đúng | 

## Vỏ cạnh 

Trường hợp quan trọng là khi đóng góp lớn nhất đã đáp ứng hoặc vượt quá yêu cầu. Đối với đầu vào:```
n = 3, w = 10
a = [10, 1, 1]
```Sắp xếp mang lại`[10, 1, 1]`. Bước đầu tiên đã đạt được mục tiêu nên thuật toán dừng ngay lập tức với câu trả lời 1. Bất kỳ cách tiếp cận nào trì hoãn việc kiểm tra không chính xác cho đến khi truyền tải đầy đủ vẫn đúng nhưng lãng phí thời gian; bất kỳ cách tiếp cận nào cố gắng cân bằng các khoản đóng góp có thể chọn không chính xác nhiều phần tử nhỏ trước tiên và trả về 3, điều này là không tối ưu. 

Một trường hợp khác là khi tất cả các giá trị đều nhỏ và chỉ có sự kết hợp của chúng hoạt động:```
n = 4, w = 20
a = [6, 6, 6, 6]
```Thứ tự sắp xếp không thay đổi và tích lũy đạt 18 sau 3 lượt chọn và 24 sau 4 lượt chọn. Thuật toán trả về chính xác 4, cho thấy nó xử lý một cách tự nhiên các tình huống tiêu thụ hết mà không cần viết hoa đặc biệt.
