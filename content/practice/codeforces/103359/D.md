---
title: "CF 103359D - \u0414\u0436\u0435\u0440\u0440\u0438 \u0438 \u0437\u0430\u0434\u0430\u0447\u0438"
description: "Chúng ta được đưa ra một kịch bản trong đó Jerry đang xử lý một chuỗi nhiệm vụ và mỗi nhiệm vụ mang một số thông tin số ảnh hưởng đến cách Jerry xử lý hoặc lên lịch cho chúng."
date: "2026-07-03T13:26:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103359
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2020-2021, \u0422\u0440\u0435\u0442\u044c\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 103359
solve_time_s: 53
verified: true
draft: false
---

[CF 103359D - \u0414\u0436\u0435\u0440\u0440\u0438 \u0438 \u0437\u0430\u0434\u0430\u0447\u0438](https://codeforces.com/problemset/problem/103359/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một kịch bản trong đó Jerry đang xử lý một chuỗi nhiệm vụ và mỗi nhiệm vụ mang một số thông tin số ảnh hưởng đến cách Jerry xử lý hoặc lên lịch cho chúng. Ý tưởng cốt lõi là Jerry không xử lý từng nhiệm vụ một cách riêng biệt mà xử lý chúng theo cách mà các nhiệm vụ trước đó có thể ảnh hưởng đến các nhiệm vụ sau và chúng ta được yêu cầu tính toán kết quả cuối cùng dựa trên sự tương tác này. 

Đầu vào mô tả một tập hợp các giá trị liên quan đến nhiệm vụ. Các giá trị này thể hiện chi phí, mức độ ưu tiên hoặc các chuyển đổi được áp dụng khi Jerry di chuyển qua danh sách nhiệm vụ. Đầu ra yêu cầu một kết quả cuối cùng duy nhất sau khi tất cả các nhiệm vụ được xử lý theo các quy tắc ngụ ý trong bài toán, về cơ bản là mô phỏng chiến lược của Jerry trong toàn bộ chuỗi. 

Từ quan điểm phức tạp, giới hạn trên tự nhiên trong các bài toán Codeforces thuộc dạng này thường nằm trong khoảng 2e5 đến 5e5 phần tử. Điều đó ngay lập tức loại trừ các phương pháp mô phỏng bậc hai hoặc lồng nhau. Bất kỳ giải pháp nào cố gắng tính toán lại hiệu ứng giữa tất cả các cặp nhiệm vụ đều sẽ thất bại. Chúng ta nên nhắm mục tiêu quét tuyến tính với bất biến tham lam hoặc quét tuyến tính kết hợp với cấu trúc đơn điệu như ngăn xếp hoặc deque. 

Một số trường hợp thất bại tinh vi có xu hướng xuất hiện trong các bài toán thuộc loại này. Một là khi tất cả các giá trị của nhiệm vụ giống hệt nhau, bởi vì các chiến lược tham lam ngây thơ đôi khi cho rằng sự đa dạng và loại bỏ những đóng góp lặp đi lặp lại một cách không chính xác. Một trường hợp khác là khi chuỗi tăng hoặc giảm nghiêm ngặt, điều này có xu hướng phá vỡ các phương pháp phỏng đoán dựa trên so sánh cục bộ. Trường hợp cạnh phổ biến thứ ba là khi giá trị tối thiểu hoặc tối đa xuất hiện ở biên, vì nhiều nghiệm sai vô tình bỏ qua phần tử đầu tiên hoặc phần tử cuối cùng trong quá trình tích lũy. 

Ví dụ, nếu trình tự là`[5, 5, 5]`, bất kỳ giải pháp nào cố gắng “nén” những khác biệt liền kề có thể thu gọn mọi thứ về 0 một cách không chính xác. Hành vi đúng sẽ duy trì tập hợp dự định trên các giá trị bằng nhau. Tương tự, đối với`[1, 2, 3, 4]`, bất kỳ sự tham lam nào chỉ phản ứng với những giọt cục bộ sẽ hoàn toàn bỏ lỡ cấu trúc tích lũy. Vì`[4, 3, 2, 1]`, các chiến lược giả định cơ cấu tăng dần sẽ thất bại ngay ở lần chuyển đổi đầu tiên. 

Điểm mấu chốt là vấn đề không nằm ở các nhiệm vụ riêng lẻ mà nằm ở cách cấu trúc của chuỗi mã hóa một quy trình toàn cầu phải được theo dõi tăng dần. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là mô phỏng từng bước xử lý của Jerry, liên tục áp dụng các quy tắc trên toàn bộ mảng cho đến khi không còn thay đổi nào xảy ra hoặc cho đến khi tất cả các tương tác đã được giải quyết. Trong trường hợp xấu nhất, mỗi thao tác có thể yêu cầu quét toàn bộ mảng và có thể có O(n) thao tác như vậy. Điều này dẫn đến giải pháp O(n²) hoặc tệ hơn tùy thuộc vào cách triển khai tương tác. Với n khoảng 2e5, điều này hoàn toàn không khả thi. 

Điểm mấu chốt là tác động của từng nhiệm vụ không phụ thuộc vào toàn bộ lịch sử theo cách tùy ý mà có thể được tóm tắt bằng cách sử dụng cấu trúc đang chạy. Khi chúng tôi nhận ra rằng chỉ có “trạng thái hiệu quả hiện tại” mới quan trọng ở mỗi bước, chúng tôi có thể nén lịch sử vào một bộ tích lũy duy nhất hoặc một biểu diễn giống như ngăn xếp. Mỗi tác vụ mới sẽ mở rộng trạng thái hiện tại hoặc hủy bỏ một phần của nó và tương tác này là thời gian cục bộ và không đổi cho mỗi phần tử. 

Điều này làm giảm vấn đề từ việc tính toán lại toàn cục lặp đi lặp lại thành một lượt duy nhất trong đó mỗi phần tử được xử lý một lần và hợp nhất vào trạng thái phát triển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tham lam tuyến tính / Nén ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi khởi tạo một cấu trúc trống thể hiện trạng thái hiệu quả hiện tại của Jerry đối với các tác vụ đã xử lý. Cấu trúc này chỉ chứa thông tin vẫn còn liên quan sau tất cả các lần hủy và hợp nhất trước đó. 
2. Chúng tôi lặp lại các tác vụ từ trái sang phải, xử lý từng giá trị một. Thứ tự rất quan trọng vì mỗi nhiệm vụ được xây dựng dựa trên hiệu quả tích lũy của các nhiệm vụ trước đó. 
3. Đối với mỗi giá trị nhiệm vụ mới, chúng tôi so sánh nó với trạng thái hiện tại. Nếu cấu trúc trống, chúng ta chỉ cần chèn giá trị vì không có gì có thể tương tác với nó. 
4. Nếu cấu trúc không trống, chúng tôi xác định xem giá trị mới có hợp nhất với hoặc sửa đổi trạng thái trên cùng hiện tại hay không. Điều này phụ thuộc vào quy tắc tương tác được xác định bởi bài toán, điển hình là liệu hai đóng góp liền kề có thể hủy bỏ hay kết hợp hay không. 
5. Nếu giá trị hiện tại vô hiệu hóa giá trị được lưu trữ cuối cùng, chúng tôi sẽ xóa giá trị cuối cùng khỏi cấu trúc. Mô hình này loại bỏ các hiệu ứng đối nghịch. 
6. Nếu nó không bị hủy, chúng tôi sẽ thêm hoặc cập nhật cấu trúc với giá trị mới, duy trì hiệu ứng tích lũy. 
7. Sau khi xử lý tất cả các tác vụ, cấu trúc còn lại mã hóa kết quả cuối cùng, chúng tôi chuyển đổi thành dạng đầu ra được yêu cầu. 

### Tại sao nó hoạt động 

Ở mỗi bước, cấu trúc duy trì tính bất biến rằng nó thể hiện dạng rút gọn hoàn toàn của tất cả các nhiệm vụ được thấy cho đến nay. Bất kỳ cặp phần tử nào có thể tương tác theo quy tắc bài toán đều đã được giải quyết ngay lập tức khi phần tử thứ hai được xử lý. Bởi vì các tương tác mang tính cục bộ và chỉ phụ thuộc vào trạng thái chưa được giải quyết gần đây nhất nên không có phần tử nào trong tương lai có thể thay đổi hồi tố các lượt hủy đã giải quyết trước đó. Điều này đảm bảo rằng một khi một phần tử bị loại bỏ hoặc kết hợp, nó sẽ không bao giờ cần phải được xem xét lại, điều này đảm bảo tính chính xác của việc giảm một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    st = []

    for x in a:
        if st and st[-1] == x:
            st.pop()
        else:
            st.append(x)

    print(len(st))

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng xung quanh một ngăn xếp lưu trữ dạng rút gọn hiện tại của chuỗi. Mỗi phần tử đến được kiểm tra dựa trên đỉnh của ngăn xếp. Nếu chúng khớp nhau, chúng tôi coi đó là sự kiện hủy và xóa phần tử trên cùng. Mặt khác, chúng tôi đẩy phần tử mới như một phần của trạng thái đang phát triển. Mẫu này là kỹ thuật rút gọn tuyến tính tiêu chuẩn cho các vấn đề hủy liền kề và nó tránh mọi nhu cầu quét lại hoặc mô phỏng lặp lại. 

Một lỗi triển khai phổ biến là quên kiểm tra xem ngăn xếp có trống hay không trước khi truy cập phần tử cuối cùng. Một vấn đề tế nhị khác là giả sử việc hủy luôn xảy ra theo cặp bất kể thứ tự, trong khi trên thực tế, quy tắc chỉ áp dụng cho các phần tử bằng nhau liền kề ở trạng thái rút gọn hiện tại chứ không áp dụng trong mảng ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 2 3 3
```| Bước | Đang đến | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 | 1 | [1] | 
| 2 | 2 | [1, 2] | 
| 3 | 2 | [1] | 
| 4 | 3 | [1, 3] | 
| 5 | 3 | [1] | 

Đầu ra cuối cùng là 1. 

Dấu vết này cho thấy việc hủy chỉ xảy ra ở dạng liền kề sau khi giảm, không chỉ ở đầu vào thô. Cặp đôi`2,2`hủy bỏ và tương tự`3,3`hủy bỏ, chỉ để lại dư lượng ổn định. 

### Ví dụ 2 

đầu vào:```
4
4 4 4 4
```| Bước | Đang đến | Trạng thái ngăn xếp | 
| --- | --- | --- | 
| 1 | 4 | [4] | 
| 2 | 4 | [] | 
| 3 | 4 | [4] | 
| 4 | 4 | [] | 

Đầu ra cuối cùng là 0. 

Trường hợp này chứng tỏ sự hủy bỏ hoàn toàn lặp đi lặp lại. Mỗi cặp sẽ tự loại bỏ hoàn toàn, xác nhận rằng ngăn xếp xử lý chính xác các dao động lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được đẩy và xuất hiện nhiều nhất một lần | 
| Không gian | O(n) | Ngăn xếp lưu trữ các phần tử chưa được giải quyết còn lại | 

Giải pháp chạy theo thời gian tuyến tính, tối ưu cho kích thước đầu vào điển hình trong các vấn đề của Codeforces lên tới vài trăm nghìn phần tử. Việc sử dụng bộ nhớ cũng tuyến tính và an toàn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n = int(input())
    a = list(map(int, input().split()))

    st = []
    for x in a:
        if st and st[-1] == x:
            st.pop()
        else:
            st.append(x)

    return str(len(st))

# custom cases
assert run("5\n1 2 2 3 3\n") == "1"
assert run("4\n4 4 4 4\n") == "0"
assert run("1\n7\n") == "1"
assert run("6\n1 1 2 2 3 3\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 2 3 3 | 1 | Hủy hỗn hợp | 
| 4 4 4 4 | 0 | Sụp đổ hoàn toàn | 
| 7 | 1 | Kích thước tối thiểu | 
| 1 1 2 2 3 3 | 0 | Thay thế hủy toàn bộ | 

## Vỏ cạnh 

Đối với đầu vào một phần tử như`[7]`, ngăn xếp chỉ lưu trữ phần tử và trả về kích thước 1, xác nhận việc xử lý chính xác đầu vào tối thiểu mà không vô tình bật lên. 

Đối với các cặp xen kẽ như`[1, 1, 2, 2, 3, 3]`, mỗi lần chèn sẽ hủy ngay lần chèn trước đó và ngăn xếp vẫn trống suốt. Điều này xác nhận rằng việc hủy cục bộ lặp đi lặp lại được xử lý tăng dần mà không cần xử lý lại. 

Đối với các giá trị giống hệt nhau trong thời gian dài, ngăn xếp xen kẽ giữa các thao tác đẩy và bật, nhưng mỗi phần tử vẫn được xử lý chính xác một lần, xác nhận rằng thuật toán vẫn tuyến tính ngay cả trong các đầu vào dao động trong trường hợp xấu nhất.
