---
title: "CF 103361N - \u041f\u043e\u0434\u0433\u043e\u0442\u043e\u0432\u043a\u0430 \u043a \u041d\u043e\u0432\u043e\u043c\u0443 \u0433\u043e\u0434\u0443"
description: "Chúng ta được tặng một bộ sưu tập nhỏ cây Giáng sinh, mỗi cây có chiều cao nguyên dương. Nhiệm vụ là chọn ba cây khác nhau sao cho cả ba cây đều có chiều cao bằng nhau. Đầu ra là chỉ số của ba cây đó."
date: "2026-07-03T13:09:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103361
codeforces_index: "N"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u041a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 103361
solve_time_s: 53
verified: true
draft: false
---

[CF 103361N - \u041f\u043e\u0434\u0433\u043e\u0442\u043e\u0432\u043a\u0430 \u043a \u041d\u043e\u0432\u043e\u043c\u0443 \u0433\u043e\u0434\u0443](https://codeforces.com/problemset/problem/103361/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập nhỏ cây Giáng sinh, mỗi cây có chiều cao nguyên dương. Nhiệm vụ là chọn ba cây khác nhau sao cho cả ba cây đều có chiều cao bằng nhau. Đầu ra là chỉ số của ba cây đó. Nếu không có bộ ba như vậy tồn tại thì chúng tôi báo cáo rằng điều đó là không thể. 

Kích thước đầu vào rất nhỏ, nhiều nhất là 100 cây và mỗi chiều cao cũng bị giới hạn bởi 100. Điều này ngay lập tức cho chúng ta biết rằng ngay cả các phương pháp bậc hai hoặc bậc ba tương đối chậm cũng có thể chấp nhận được, vì tổng số thao tác trong trường hợp xấu nhất là theo thứ tự 10^6 hoặc 10^7, điều này không đáng kể đối với giới hạn 2 giây trong Python. 

Một kiểu thất bại tinh vi xuất hiện khi một người muốn chọn “bất kỳ độ cao lặp lại nào”. Có ít nhất hai chiều cao bằng nhau là không đủ. Ví dụ, nếu chiều cao là`[5, 5, 7]`, có sự lặp lại nhưng không đủ số bản sao để tạo thành bộ ba nên đáp án vẫn phải là`-1`. Điều kiện nghiêm ngặt là một số giá trị xuất hiện ít nhất ba lần. 

Một tình huống khó khăn khác là khi tồn tại nhiều câu trả lời hợp lệ. Bất kỳ bộ ba chỉ số riêng biệt hợp lệ nào cũng có thể được chấp nhận, vì vậy thuật toán không nên suy nghĩ quá nhiều về việc lựa chọn hoặc cố gắng tối ưu hóa theo thứ tự từ điển. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là kiểm tra từng bộ ba chỉ số`(i, j, k)`và kiểm tra xem`h[i] == h[j] == h[k]`. Điều này đơn giản và chính xác vì nó xác minh rõ ràng tất cả các bộ ba có thể có. Tuy nhiên, chi phí của nó tỷ lệ thuận với số lượng bộ ba, gần bằng`n^3 / 6`. Với`n = 100`, đây là khoảng 160.000 lần lặp, điều này thực sự vẫn ổn trong Python, nhưng nó nặng một cách không cần thiết do tính đơn giản của cấu trúc. 

Quan sát quan trọng là chúng ta không quan tâm đến bộ ba nào chúng ta chọn ngoài sự bằng nhau về giá trị. Chúng ta chỉ cần biết liệu một số giá trị có xuất hiện ít nhất ba lần hay không và nếu có thì chúng ta cần ba chỉ số bất kỳ nơi giá trị đó xuất hiện. Điều này biến vấn đề từ việc tìm kiếm sự kết hợp của các chỉ số thành một nhiệm vụ ghi sổ và tần suất. 

Thay vì tạo ra các bộ ba, chúng tôi theo dõi vị trí của từng độ cao. Ngay khi chúng tôi tìm thấy độ cao với ba vị trí đã ghi, chúng tôi có thể xuất chúng ngay lập tức. Điều này làm giảm vấn đề xuống còn một lần truyền qua mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bộ ba vũ phu | O(n^3) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tần suất có chỉ số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ánh xạ từ mỗi độ cao đến danh sách các chỉ mục nơi nó xuất hiện. 

1. Lặp lại danh sách các cây từ trái sang phải đồng thời ghi lại các chỉ số cho từng chiều cao. Điều này đảm bảo chúng tôi duy trì thứ tự xuất hiện tự nhiên, giúp đưa ra các chỉ số hợp lệ ngay lập tức. 
2. Bất cứ khi nào chúng tôi thêm một chỉ mục mới vào danh sách có chiều cao cụ thể, hãy kiểm tra xem kích thước danh sách đã đạt đến 3 hay chưa. Khi nó đạt tới 3, chúng tôi có thể ngừng xử lý thêm vì chúng tôi đã có câu trả lời hợp lệ. 
3. Xuất ba chỉ số được lưu trữ đó theo thứ tự bất kỳ và kết thúc chương trình. 
4. Nếu chúng tôi quét xong tất cả các cây mà không có bất kỳ chiều cao nào đạt tần số 3, thì xuất ra`-1`. 

Lựa chọn thiết kế chính là thoát sớm ở bước 2. Không cần phải xây dựng đầy đủ bảng tần số sau khi tìm thấy bộ ba hợp lệ. 

### Tại sao nó hoạt động 

Đối với mỗi giá trị độ cao, chúng tôi duy trì chính xác tập hợp các chỉ số nơi nó xuất hiện. Nếu bất kỳ chiều cao nào xuất hiện ít nhất ba lần thì ba lần xuất hiện đó tạo thành một giải pháp hợp lệ theo định nghĩa của vấn đề. Vì chúng tôi chỉ lưu trữ các chỉ số thực tế từ dữ liệu đầu vào nên chúng tôi không bao giờ giả tạo hoặc sao chép các vị trí. Do đó, thuật toán không thể xuất ra các chỉ số không hợp lệ hoặc độ cao không khớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    h = list(map(int, input().split()))
    
    pos = {}
    
    for i, val in enumerate(h, start=1):
        if val not in pos:
            pos[val] = []
        pos[val].append(i)
        if len(pos[val]) == 3:
            print(pos[val][0], pos[val][1], pos[val][2])
            return
    
    print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng từ điển được khóa theo chiều cao để lưu trữ các chỉ mục. Mỗi chỉ số dựa trên 1 để phù hợp với các quy ước lập trình cạnh tranh điển hình. Việc quay lại sớm đảm bảo chúng tôi dừng lại ngay sau khi tìm thấy bộ ba hợp lệ. Điểm tinh tế duy nhất là đảm bảo chúng tôi nối thêm trước khi kiểm tra điều kiện độ dài để chỉ mục hiện tại được đưa vào bộ ba ứng cử viên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 2 3 2 1 2
```Chúng tôi theo dõi các lần xuất hiện: 

| tôi | chiều cao | vị trí được lưu trữ sau bước | hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | [1] | tiếp tục | 
| 2 | 2 | [2] | tiếp tục | 
| 3 | 3 | [3] | tiếp tục | 
| 4 | 2 | [2, 4] | tiếp tục | 
| 5 | 1 | [1, 5] | tiếp tục | 
| 6 | 2 | [2, 4, 6] | đầu ra | 

Tại chỉ số 6, chiều cao 2 đạt đến ba lần xuất hiện, vì vậy chúng ta xuất ra`2 4 6`. 

Điều này xác nhận tính bất biến rằng chúng tôi luôn duy trì vị trí xuất hiện chính xác, vì vậy, lần đầu tiên danh sách đạt kích thước 3, đó là câu trả lời hợp lệ. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
```| tôi | chiều cao | vị trí được lưu trữ | 
| --- | --- | --- | 
| 1 | 1 | [1] | 
| 2 | 2 | [2] | 
| 3 | 3 | [3] | 
| 4 | 4 | [4] | 
| 5 | 5 | [5] | 

Không có danh sách nào đạt đến kích thước 3, vì vậy thuật toán kết thúc quá trình quét và xuất ra`-1`. 

Điều này cho thấy hành vi dự phòng khi không tồn tại bộ ba hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cây được xử lý một lần, với O(1) thao tác từ điển trung bình mỗi bước | 
| Không gian | O(n) | Trong trường hợp xấu nhất, tất cả các chỉ mục đều được lưu trữ trong bản đồ | 

Các ràng buộc n 100 làm cho giải pháp này cực kỳ nhanh trong thực tế, nhưng ngay cả khi n lớn hơn nhiều thì cấu trúc tuyến tính tương tự vẫn có hiệu lực. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    
    solve()
    
    out = sys.stdout.getvalue().strip()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

def solve():
    input = sys.stdin.readline
    n = int(input())
    h = list(map(int, input().split()))
    
    pos = {}
    for i, val in enumerate(h, start=1):
        pos.setdefault(val, []).append(i)
        if len(pos[val]) == 3:
            print(pos[val][0], pos[val][1], pos[val][2])
            return
    print(-1)

# provided samples
assert run("6\n1 2 3 2 1 2\n") in ["2 4 6", "6 4 2"]
assert run("5\n1 2 3 4 5\n") == "-1"

# custom cases
assert run("3\n7 7 7\n") in ["1 2 3", "2 1 3", "3 2 1"], "all equal minimum triple"
assert run("4\n1 1 1 1\n") in ["1 2 3", "1 3 2"], "more than three occurrences"
assert run("6\n5 4 5 4 5 4\n") in ["1 3 5", "2 4 6"], "multiple valid groups"
assert run("1\n10\n") == "-1", "minimum n no triple"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bằng 3 | hoán vị bất kỳ của 1 2 3 | trường hợp thành công cơ bản | 
| tất cả đều bằng 4 | bất kỳ bộ ba nào trong số 3 bộ đầu tiên | trùng lặp bổ sung | 
| xen kẽ | 1 3 5 hoặc 2 4 6 | nhiều lựa chọn hợp lệ | 
| phần tử đơn | -1 | ranh giới tối thiểu | 

## Vỏ cạnh 

Khi tất cả các cây có cùng chiều cao, ba chỉ số đầu tiên ngay lập tức tạo thành câu trả lời hợp lệ. Thuật toán xử lý việc này mà không cần quét phần còn lại của mảng sau khi đạt đến lần xuất hiện thứ ba. 

Khi có chính xác hai lần xuất hiện ở mọi độ cao, không có danh sách nào đạt tới kích thước ba, do đó quá trình quét hoàn tất và xuất ra chính xác`-1`. 

Khi bộ ba hợp lệ xuất hiện muộn trong mảng, thuật toán vẫn phát hiện ra nó vì nó liên tục tích lũy các chỉ số và không phụ thuộc vào bất kỳ giả định thứ tự nào ngoài việc xử lý tuần tự.
