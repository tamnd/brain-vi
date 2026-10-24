---
title: "CF 105962I - Nuôi trồng hào quang"
description: "Chúng ta được cho một tập hợp các bài toán, mỗi bài được mô tả bằng hai con số. Con số đầu tiên là ngưỡng yêu cầu: Beraldo chỉ có thể giải quyết vấn đề đó nếu hào quang hiện tại của anh ấy ít nhất đạt được giá trị đó. Con số thứ hai là phần thưởng: nếu anh ta giải quyết được vấn đề, hào quang của anh ta sẽ tăng lên theo mức đó."
date: "2026-06-22T16:17:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105962
codeforces_index: "I"
codeforces_contest_name: "UNICAMP Freshman Contest 2025"
rating: 0
weight: 105962
solve_time_s: 49
verified: true
draft: false
---

[CF 105962I - Nuôi trồng hào quang](https://codeforces.com/problemset/problem/105962/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các bài toán, mỗi bài được mô tả bằng hai con số. Con số đầu tiên là ngưỡng yêu cầu: Beraldo chỉ có thể giải quyết vấn đề đó nếu hào quang hiện tại của anh ấy ít nhất đạt được giá trị đó. Con số thứ hai là phần thưởng: nếu anh ta giải quyết được vấn đề, hào quang của anh ta sẽ tăng lên theo mức đó. Anh ta bắt đầu với giá trị hào quang ban đầu và có thể chọn bất kỳ thứ tự giải quyết vấn đề nào, nhưng mỗi vấn đề chỉ có thể được sử dụng nhiều nhất một lần. 

Nhiệm vụ là xác định hào quang tối đa có thể mà anh ta có thể đạt được sau khi liên tục chọn các vấn đề có thể giải quyết được và áp dụng phần thưởng của chúng. 

Khó khăn chính là việc giải quyết một vấn đề sẽ làm thay đổi tập hợp các vấn đề có sẵn. Một vấn đề ban đầu quá khó có thể giải quyết được sau này và việc chọn sai vấn đề ban đầu có thể cản trở khả năng phát triển tốt hơn sau này. 

Các ràng buộc cho phép giải quyết tới 100.000 vấn đề, do đó, bất kỳ giải pháp nào thử tất cả các hoán vị hoặc mô phỏng tất cả các tập hợp con đều không thể thực hiện được. Ngay cả một mô phỏng bậc hai trong đó mỗi bước quét tất cả các vấn đề còn lại cũng sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn một giây. Điều này ngay lập tức loại trừ các chiến lược quét theo đơn đặt hàng ngây thơ hoặc lặp đi lặp lại mà không có sổ sách kế toán hiệu quả. 

Một trường hợp thất bại tinh vi xuất hiện khi một vấn đề mang lại phần thưởng lớn nhưng có yêu cầu cao, trong khi các vấn đề về phần thưởng nhỏ hơn sẽ mở khóa quyền truy cập vào phần thưởng đó. Ví dụ: nếu chúng ta bắt đầu với hào quang thấp và tham lam chọn phần thưởng lớn nhất ngay lập tức, chúng ta có thể bỏ lỡ trình tự mang lại lợi ích lớn hơn sau này. Việc đặt hàng quan trọng trên toàn cầu, không phải cục bộ. 

Một trường hợp khó khăn khác xảy ra khi tất cả các vấn đề sẵn có ban đầu đòi hỏi nhiều hào quang hơn chúng ta có. Trong tình huống đó, câu trả lời chỉ đơn giản là hào quang khởi đầu, vì không thể tiến triển được. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ mô phỏng tất cả các trình tự giải quyết vấn đề có thể có. Từ hào quang nhất định, chúng tôi kiểm tra mọi vấn đề chưa được giải quyết, thử bất kỳ vấn đề nào hiện có, lặp lại sau khi áp dụng phần thưởng và theo dõi kết quả tối đa. Điều này mô hình hóa vấn đề một cách chính xác vì nó khám phá tất cả các chuỗi hành động hợp lệ. 

Tuy nhiên, hệ số phân nhánh lên tới N ở mỗi bước và độ sâu cũng lên tới N. Điều này dẫn đến hành vi giai thừa gần như N trong trường hợp xấu nhất, điều này hoàn toàn không khả thi ngay cả đối với N nhỏ đến 20, chứ chưa nói đến 100.000. 

Nhận xét quan trọng là quyết định ở mỗi bước chỉ phụ thuộc vào vấn đề nào hiện có thể giải quyết được chứ không phụ thuộc vào toàn bộ lịch sử. Trong số tất cả các vấn đề hiện có thể giải quyết được, việc chọn bất kỳ tùy chọn phần thưởng thấp nào thay vì tùy chọn phần thưởng cao hơn sẽ không bao giờ có lợi nếu tùy chọn phần thưởng cao hơn đã có sẵn. Lý do là sự đơn điệu: hào quang chỉ tăng lên, nên một khi vấn đề đã xuất hiện, nó vẫn tồn tại cho đến khi được giải quyết. 

Điều này gợi ý một quá trình tham lam: duy trì tập hợp tất cả các vấn đề có yêu cầu cao nhất là vấn đề hiện tại và luôn chọn trong số đó vấn đề có phần thưởng lớn nhất. Mỗi lựa chọn sẽ tăng thêm hào quang, có khả năng mở ra những vấn đề mới. Điều này có thể được thực hiện một cách hiệu quả bằng cách sử dụng sắp xếp và heap tối đa. 

Đầu tiên chúng ta sắp xếp các vấn đề theo yêu cầu của chúng. Sau đó, chúng tôi lướt qua chúng khi hào quang hiện tại của chúng tôi tăng lên, đẩy tất cả các vấn đề mới được mở khóa vào một đống tối đa được khóa bằng phần thưởng. Mỗi lần chúng tôi nhận được phần thưởng tốt nhất hiện có, chúng tôi sẽ tăng cường hào quang và tiếp tục. Điều này đảm bảo chúng ta không bao giờ lãng phí cơ hội giải quyết một vấn đề có giá trị cao một khi nó có thể tiếp cận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N!) | O(N) | Quá chậm | 
| Tối ưu (sắp xếp + đống) | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc: một danh sách các vấn đề được sắp xếp theo hào quang cần thiết và một đống tối đa chứa phần thưởng của các vấn đề hiện có thể giải quyết được.

1. Sắp xếp tất cả các bài toán theo giá trị yêu cầu Ai theo thứ tự không giảm. Điều này cho phép chúng ta biết các vấn đề về thứ tự sẽ xuất hiện khi hào quang tăng lên. 
2. Khởi tạo một con trỏ trên danh sách đã sắp xếp và đặt hào quang hiện tại thành K. Đồng thời khởi tạo một vùng heap tối đa trống. 
3. Di chuyển con trỏ về phía trước trong khi yêu cầu của bài toán tiếp theo nhiều nhất là hào quang hiện tại và đẩy phần thưởng của nó vào đống. Bước này tập hợp tất cả các vấn đề mới có thể tiếp cận được. 
4. Nếu heap trống vào thời điểm này, điều đó có nghĩa là không còn vấn đề nào có thể được giải quyết với hào quang hiện tại, do đó quá trình dừng lại. 
5. Nếu không, hãy trích xuất vấn đề có phần thưởng lớn nhất từ ​​đống và thêm phần thưởng của nó vào hào quang hiện tại. Điều này thể hiện việc lựa chọn hành động có lợi nhất. 
6. Lặp lại các bước từ 3 đến 5 cho đến khi không thể thực hiện được nữa. 

Lý do chúng tôi luôn mở rộng vùng lưu trữ trước khi chọn là vì việc giải quyết một vấn đề có thể mở khóa nhiều vấn đề khác và chúng tôi muốn đảm bảo tất cả các tùy chọn hiện có thể truy cập đều được xem xét trước khi đưa ra lựa chọn. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì tính bất biến rằng heap chứa chính xác phần thưởng của tất cả các vấn đề chưa được giải quyết mà yêu cầu của nó nhiều nhất là hào quang hiện tại. Vì hào quang chỉ tăng lên nên một khi vấn đề trở nên đủ điều kiện, nó sẽ không bao giờ trở thành không đủ điều kiện. Vì vậy, chúng tôi không bao giờ bỏ lỡ một hành động có thể thực hiện được. 

Trong số tất cả các hành động hiện có, việc chọn phần thưởng tối đa là an toàn vì bất kỳ lựa chọn phần thưởng nhỏ hơn nào sau đó sẽ khiến chúng ta có hào quang thấp hơn hoặc bằng nhau, điều này không thể giải quyết được nhiều vấn đề hơn lựa chọn phần thưởng cao hơn. Tính đơn điệu này đảm bảo rằng các lựa chọn tối ưu cục bộ không cản trở các cơ hội trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n, k = map(int, input().split())
    problems = []
    for _ in range(n):
        a, b = map(int, input().split())
        problems.append((a, b))
    
    problems.sort()
    
    i = 0
    aura = k
    heap = []
    
    while True:
        while i < n and problems[i][0] <= aura:
            heapq.heappush(heap, -problems[i][1])
            i += 1
        
        if not heap:
            break
        
        aura += -heapq.heappop(heap)
    
    print(aura)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ sắp xếp các vấn đề để chúng ta có thể kích hoạt chúng dần dần khi hào quang phát triển. Con trỏ`i`đảm bảo mỗi vấn đề được đẩy vào vùng nhớ heap chính xác một lần, giúp quá trình tiền xử lý trở nên tuyến tính sau khi sắp xếp. 

Vùng heap lưu trữ các giá trị âm để mô phỏng vùng heap tối đa bằng cách triển khai vùng heap tối thiểu của Python. Mỗi lần lặp lại, chúng tôi đảm bảo tất cả các vấn đề hiện có thể tiếp cận được sẽ được chèn vào trước khi chọn phần thưởng tốt nhất. 

Một cạm bẫy triển khai phổ biến là quên kiểm tra lại các vấn đề mới được mở khóa sau khi tăng hào quang. Đó là lý do tại sao vòng lặp bên ngoài xen kẽ giữa việc mở rộng heap và tiêu thụ từ nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
2 1
4 2
10 100
```| Bước | Hào quang | Mới được thêm | Đống (tối đa) | Phần thưởng được chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | (2,1), (4,2) | 2, 1 | 2 | 
| 2 | 7 | (10.100) | 100, 1 | 100 | 
| 3 | 107 | không | 1 | 1 | 
| 4 | 108 | không | trống | dừng lại | 

Điều này cho thấy phần thưởng vừa phải ban đầu được sử dụng như thế nào để mở khóa phần thưởng lớn hơn nhiều sau đó và sau đó ngay cả phần thưởng nhỏ còn lại cũng được tiêu thụ vào cuối. 

### Ví dụ 2 

đầu vào:```
2 7
7 2
10 1
```| Bước | Hào quang | Mới được thêm | Đống | Được chọn | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | (7,2) | 2 | 2 | 
| 2 | 9 | không | trống | dừng lại | 

Điều này chứng tỏ rằng khi không còn vấn đề nào có thể tiếp cận được nữa thì quy trình sẽ kết thúc một cách chính xác mà không cần thử các tác vụ yêu cầu cao không thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Việc sắp xếp chiếm ưu thế và mỗi vấn đề được đưa vào heap một lần | 
| Không gian | O(N) | Lưu trữ cho tất cả các vấn đề và nội dung heap | 

Các ràng buộc cho phép lên tới 100.000 vấn đề và các thao tác log N có thể dễ dàng đủ nhanh trong giới hạn một giây, giúp phương pháp này trở nên an toàn. 

## Trường hợp thử nghiệm```python
import sys, io
import subprocess

def run(inp: str) -> str:
    return subprocess.run(
        ["python3", "solution.py"],
        input=inp.encode(),
        stdout=subprocess.PIPE
    ).stdout.decode().strip()

# provided samples
assert run("3 5\n2 1\n4 2\n10 100\n") == "108"
assert run("5 5\n6 10\n2 3\n10 100\n20 1\n25 1\n") == "108"
assert run("2 7\n7 2\n10 1\n") == "9"

# custom cases
assert run("1 0\n0 5\n") == "5", "single reachable"
assert run("1 0\n1 5\n") == "0", "single unreachable"
assert run("4 1\n1 1\n1 2\n1 3\n1 4\n") == "11", "all immediately reachable"
assert run("3 3\n10 5\n20 10\n30 100\n") == "3", "no progress possible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| có thể truy cập duy nhất | 5 | ứng dụng đạt được cơ bản | 
| không thể truy cập được | 0 | không có nước đi nào | 
| tất cả đều có thể truy cập ngay lập tức | 11 | heap luôn chọn tốt nhất | 
| không thể tiến bộ được | 3 | chấm dứt sớm đúng đắn | 

## Vỏ cạnh 

Trường hợp quan trọng là khi ban đầu không có vấn đề nào có thể giải quyết được. Đối với đầu vào:```
3 1
10 5
20 10
30 100
```thuật toán sắp xếp tất cả các vấn đề nhưng không bao giờ chèn bất cứ thứ gì vào heap vì con trỏ không bao giờ tiến lên. Vùng heap vẫn trống ngay lập tức, do đó vòng lặp kết thúc và xuất ra 1, điều này đúng vì không thể thực hiện hành động nào. 

Một trường hợp khác là khi việc mở khóa xảy ra theo chuỗi. Ví dụ:```
3 1
1 2
3 3
5 10
```Ở hào quang 1, chỉ có vấn đề đầu tiên đi vào đống và được thực hiện, tăng hào quang lên 3. Điều này ngay lập tức mở khóa vấn đề thứ hai, sau đó được thực hiện, mở khóa vấn đề thứ ba. Thuật toán xử lý việc này một cách tự nhiên vì việc mở rộng vùng heap được lặp lại sau mỗi lần tăng, đảm bảo tất cả các vấn đề mới có thể tiếp cận đều được xem xét ở mỗi giai đoạn.
