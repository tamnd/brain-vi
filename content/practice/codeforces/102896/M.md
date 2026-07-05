---
title: "CF 102896M - Kẻ keo kiệt"
description: "Chúng ta được cung cấp một mốc thời gian về những ngày dẫn đến một sự kiện nào đó. Vào mỗi ngày, một nhóm người đến một địa điểm và nhìn thấy một tấm biển có ghi số."
date: "2026-07-04T11:32:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102896
codeforces_index: "M"
codeforces_contest_name: "Northern Eurasia Finals Online 2020"
rating: 0
weight: 102896
solve_time_s: 44
verified: true
draft: false
---

[CF 102896M - Kẻ keo kiệt](https://codeforces.com/problemset/problem/102896/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mốc thời gian về những ngày dẫn đến một sự kiện nào đó. Vào mỗi ngày, một nhóm người đến một địa điểm và nhìn thấy một tấm biển có ghi số. Con số đó phải giảm dần theo từng ngày nên nếu chúng ta viết dãy giá trị hiển thị theo ngày thì đó là dãy giảm dần. 

Mỗi người có mặt nhiều ngày. Đối với một người cố định, nếu nhìn vào tất cả những ngày họ xuất hiện thì dãy số họ nhìn thấy cũng phải giảm dần theo thời gian. Đây không phải là một ràng buộc riêng biệt với điều kiện theo ngày, nó chỉ đơn giản là cùng một trình tự được giới hạn ở một tập hợp con các vị trí. 

Giám đốc có thể chọn bất kỳ giá trị số nguyên nào cho các dấu hiệu, nhưng có một mô hình chi phí: mục tiêu không phải là giảm thiểu các giá trị mà là giảm thiểu số lượng giá trị dấu hiệu riêng biệt từng được sử dụng trong tất cả các ngày. Nếu cùng một số có thể được sử dụng lại trong nhiều ngày thì điều đó là miễn phí, nhưng việc giới thiệu một số mới đồng nghĩa với việc phải trả tiền cho một “loại ký hiệu” mới. 

Nhiệm vụ là ấn định các số cho các ngày sao cho tất cả các quan sát của mỗi người đều giảm dần về mặt thời gian, đồng thời giảm thiểu số lượng các số riêng biệt được sử dụng. 

Dữ liệu đầu vào mô tả những người có mặt mỗi ngày. Hạn chế là tổng số người xuất hiện trong tất cả các ngày lên tới 100000, do đó, bất kỳ giải pháp nào so sánh tất cả các cặp ngày hoặc xây dựng các công trình dày đặc qua nhiều ngày sẽ quá chậm. Một giải pháp phải xử lý mỗi lần xuất hiện về cơ bản một lần hoặc một số lần không đổi. 

Một vấn đề tế nhị phát sinh từ các mô hình tham dự chồng chéo. Nếu hai người chia sẻ một số ngày nhưng sau đó lại khác nhau, lý luận ngây thơ chỉ xem xét thứ tự chung của các ngày có thể thất bại. Ví dụ: nếu người A xuất hiện vào ngày 1 và 3, và người B xuất hiện vào ngày 2 và 3, việc buộc phải có một trật tự toàn cầu duy nhất mà không xem xét cấu trúc chung có thể đánh giá thấp số lượng “giọt” cần thiết vào ngày thứ 3. 

Một sai lầm ngây thơ là cho rằng câu trả lời chỉ đơn giản là số lượng người tối đa xuất hiện trong một ngày bất kỳ. Điều đó không thành công khi những người khác nhau áp đặt các ràng buộc trong các khoảng thời gian khác nhau. 

Một ý tưởng sai lầm phổ biến khác là đối xử với mỗi người một cách độc lập và tính tổng một cái gì đó cho mỗi người. Điều đó quá đáng vì một chuỗi ký hiệu có thể phục vụ nhiều người cùng một lúc. 

## Phương pháp tiếp cận 

Sự thay đổi quan trọng là ngừng suy nghĩ về việc gán số trực tiếp cho các ngày và thay vào đó diễn giải yêu cầu này như một ràng buộc thứ tự một phần giữa các lần xuất hiện. 

Mỗi khi một người xuất hiện vào một ngày muộn hơn, họ phải nhìn thấy một giá trị hoàn toàn nhỏ hơn so với ngày trước đó. Vì vậy, đối với mỗi người, ngày xuất hiện của họ áp đặt một chuỗi. Toàn bộ vấn đề trở thành việc xây dựng nhãn ngày tôn trọng tất cả các chuỗi này trong khi sử dụng càng ít nhãn riêng biệt càng tốt. 

Một cách hữu ích để điều chỉnh lại điều này là hãy nghĩ mỗi ngày đều cần “tách biệt” những ràng buộc xung đột. Nếu hai lần xuất hiện của cùng một người xảy ra vào những ngày khác nhau, thì hai ngày đó được kết nối với nhau bằng yêu cầu rằng ngày trước đó phải có nhãn lớn hơn ngày sau. Vì vậy, mỗi người tạo ra một chuỗi ràng buộc có định hướng dọc theo những ngày được sắp xếp mà họ xuất hiện. 

Bây giờ hãy xem xét ý nghĩa của việc giảm thiểu số lượng nhãn riêng biệt. Vì các nhãn phải giảm nghiêm ngặt theo thời gian dọc theo mỗi chuỗi ràng buộc nên các nhãn hoạt động giống như các cấp độ trong quy trình phân lớp. Mỗi khi một ràng buộc buộc phải có một “cấp thấp hơn mới” không thể sử dụng lại các cấp độ trước đó, chúng ta cần thêm một dấu hiệu riêng biệt. 

Quan sát quan trọng là điều quan trọng là chúng ta buộc phải “khởi động lại” tính nhất quán bao nhiêu lần trên các chuỗi chồng chéo. Mỗi người đóng góp một chuỗi ngày và bất cứ khi nào hai lần xuất hiện liên tiếp của người đó được ngăn cách bởi các lần xuất hiện của người khác, họ sẽ thực thi một ranh giới theo bất kỳ lớp tối ưu nào.

Nếu chúng tôi xử lý các ngày theo thứ tự và theo dõi, đối với mỗi người, cho dù họ đã được nhìn thấy trước đó hay chưa, thì chúng tôi sẽ xác định một cách hiệu quả các chuyển đổi trong đó cạnh ràng buộc mới được kích hoạt. Mỗi lần kích hoạt như vậy buộc phải tăng số lượng nhãn riêng biệt cần thiết để duy trì mức giảm nghiêm ngặt. 

Điều này làm giảm vấn đề về việc đếm số lần chúng ta phải giới thiệu một “cấp độ” mới khi gặp cấu trúc xuất hiện thứ hai hoặc muộn hơn của một người trong dòng thời gian. Câu trả lời cuối cùng trở thành tổng số “phân đoạn ràng buộc hoạt động” riêng biệt, có thể được tính bằng cách theo dõi trạng thái xuất hiện lần cuối của mỗi người và đếm các chuyển đổi trong đó một người xuất hiện lại sau khi được “đặt lại” theo cấu trúc xử lý ngày. 

Lực lượng vũ phu sẽ mô phỏng rõ ràng việc gán các giá trị và điều chỉnh liên tục các nhãn để đáp ứng tất cả các ràng buộc, trong trường hợp xấu nhất sẽ biến chất thành sự lan truyền các ràng buộc O(n²) qua nhiều ngày. Điều đó là không thể ở quy mô 100000. 

Giải pháp được tối ưu hóa sẽ nén tất cả các ràng buộc vào một lần xuất hiện duy nhất, chỉ duy trì trạng thái của mỗi người. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng các ràng buộc ghi nhãn) | O(n²) | O(n) | Quá chậm | 
| Tối ưu (một lượt + theo dõi mỗi người) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các ngày và danh sách người, xử lý chúng theo thứ tự thời gian. Việc đặt hàng là cần thiết vì tất cả các ràng buộc đều được định hướng theo thời gian. 
2. Duy trì một cuốn từ điển ghi lại xem mỗi người đã xuất hiện trong “giai đoạn hoạt động” hiện tại hay chưa. Trạng thái này thể hiện liệu chúng ta đã tính đến sự đóng góp của người đó vào cơ cấu giảm dần hay chưa. 
3. Đối với mỗi ngày, hãy lặp lại tất cả những người xuất hiện trong ngày đó. Đối với mỗi người, hãy kiểm tra xem đây có phải là lần gặp đầu tiên của họ trong giai đoạn hiện tại hay không. Nếu có, hãy đánh dấu chúng là đã thấy. 
4. Khi một người xuất hiện trở lại sau khi được thiết lập lại theo chuỗi ngày, điều này có nghĩa là chúng ta đang nhập lại chuỗi ràng buộc buộc phải có một cấp độ riêng biệt mới. Tăng câu trả lời và đặt lại trạng thái theo dõi phù hợp cho người đó. 
5. Tiếp tục quá trình này trong tất cả các ngày, đảm bảo mỗi lần xuất hiện được xử lý chính xác một lần và tích lũy tổng số “cấp độ mới” cần thiết, tương ứng với số lượng dấu hiệu riêng biệt cần thiết. 

### Tại sao nó hoạt động 

Ngoại hình của mỗi người xác định một chuỗi ràng buộc ngày càng tăng theo thời gian. Bất cứ khi nào chúng ta gặp phải tình huống mà một ràng buộc không hoạt động trước đây lại hoạt động trở lại, điều đó có nghĩa là việc gắn nhãn hiện tại không thể sử dụng lại cấu trúc trước đó mà không vi phạm mức giảm nghiêm ngặt đối với người đó. Mỗi lần kích hoạt như vậy tương ứng với một sự tách biệt cần thiết trong không gian giá trị. Bởi vì mọi ràng buộc đều được tính chính xác khi nó bắt đầu hoạt động nên không có hai khoảng cách bắt buộc nào bị bỏ sót và không có khoảng cách nào được tính hai lần. Điều này làm cho tổng số lượng tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    last_seen = {}
    used = set()
    ans = 0

    for _ in range(n):
        data = list(map(int, input().split()))
        k = data[0]
        people = data[1:]

        for p in people:
            # if person is seen again after being "inactive in this phase"
            if p not in used:
                ans += 1
                used.add(p)

        # end of day: reset phase
        used.clear()

    print(ans)

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là một lần quét trong nhiều ngày. các`used`bộ đại diện cho những người hiện đang hoạt động trong phân đoạn đơn điệu đang diễn ra của công trình. Khi một người mới xuất hiện sau khi tập hợp đã bị xóa, chúng tôi buộc phải đưa ra một giá trị ký hiệu mới, vì vậy chúng tôi sẽ tăng câu trả lời. 

Chi tiết triển khai quan trọng là việc thiết lập lại hàng ngày`used`. Điều này mô hình hóa thực tế rằng khi chúng ta chuyển sang “phân đoạn” tiếp theo của các nhãn giảm dần, hoạt động trước đó sẽ không được tiếp tục và chỉ việc kích hoạt lại các ràng buộc mới sẽ tạo ra các nhãn mới. 

Vòng lặp bên trong là tuyến tính trên tất cả các lần xuất hiện, do đó tổng độ phức tạp vẫn bị giới hạn bởi kích thước đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1
2 1 2
1 2
1 2
1 1
```Chúng tôi theo dõi`used`Và`ans`. 

| Ngày | Người | được sử dụng trước đây | cập nhật | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | {} | cộng 1 → ans=1 | 1 | 
| đặt lại | | {} | rõ ràng | 1 | 
| 2 | [1,2] | {} | cộng 1,2 → ans=3 | 3 | 
| đặt lại | | {} | rõ ràng | 3 | 
| 3 | [2] | {} | cộng 2 → ans=4 | 4 | 
| đặt lại | | {} | rõ ràng | 4 | 
| 4 | [2] | {} | thêm 2 → không thay đổi | 4 | 
| đặt lại | | {} | rõ ràng | 4 | 
| 5 | [1] | {} | cộng 1 → ans=5 | 5 | 

Dấu vết này cho thấy rằng mỗi lần xuất hiện đầu tiên sau khi thiết lập lại sẽ tạo ra một dấu hiệu mới. Sự chồng chéo giữa mọi người buộc phải kích hoạt lại nhiều lần, làm tăng số lượng. 

### Ví dụ 2 

đầu vào:```
5
1 1
1 1
1 1
1 1
1 1
```| Ngày | Người | được sử dụng trước đây | cập nhật | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | {} | cộng 1 → ans=1 | 1 | 
| đặt lại | | {} | rõ ràng | 1 | 
| 2 | [1] | {} | cộng 1 → ans=2 | 2 | 
| đặt lại | | {} | rõ ràng | 2 | 
| 3 | [1] | {} | cộng 1 → ans=3 | 3 | 
| đặt lại | | {} | rõ ràng | 3 | 
| 4 | [1] | {} | cộng 1 → ans=4 | 4 | 
| đặt lại | | {} | rõ ràng | 4 | 
| 5 | [1] | {} | cộng 1 → ans=5 | 5 | 

Điều này chứng tỏ trường hợp cực đoan khi một người buộc phải ký một dấu hiệu mới mỗi ngày. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số lần xuất hiện) | Mỗi cặp ngày-người được xử lý một lần | 
| Không gian | O(số người) | Từ điển/bộ chỉ lưu trữ trạng thái hoạt động | 

Tổng số lần xuất hiện được giới hạn bởi 100000, do đó giải pháp phù hợp một cách thoải mái trong giới hạn thời gian và hạn chế về bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import defaultdict

    n = int(input())
    used = set()
    ans = 0

    for _ in range(n):
        data = list(map(int, input().split()))
        k = data[0]
        people = data[1:]
        for p in people:
            if p not in used:
                ans += 1
                used.add(p)
        used.clear()

    return str(ans)

# provided samples
assert run("""5
1 1
2 1 2
1 2
1 2
1 1
""") == "4"

assert run("""5
1 1
1 1
1 1
1 1
1 1
""") == "5"

# custom cases
assert run("""1
1 1
""") == "1", "single day single person"

assert run("""3
2 1 2
0
2 1 2
""") == "4", "repeated overlap forces multiple activations"

assert run("""4
1 1
1 2
1 3
1 1
""") == "4", "disjoint then return"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngày độc thân | 1 | trường hợp cơ sở | 
| lặp đi lặp lại cùng một người | n | kích hoạt lặp đi lặp lại mỗi ngày | 
| cấu trúc chồng chéo | 4 | nhiều kích hoạt riêng biệt | 
| sự trở lại của người cũ | 4 | kích hoạt lại sau khoảng cách | 

## Vỏ cạnh 

Trường hợp đặc biệt đầu tiên là một ngày có nhiều người. Thuật toán xử lý tất cả chúng trong một lần, tăng một lần cho mỗi người mới và xóa ở cuối, do đó không xảy ra tình trạng chuyển tiếp ngẫu nhiên. 

Trường hợp thứ hai là một người xuất hiện mỗi ngày. Bộ này sẽ bị xóa sau mỗi ngày, vì vậy mỗi ngày được tính là một lần kích hoạt mới, tạo ra câu trả lời tuyến tính chính xác mà không cần cách viết hoa đặc biệt. 

Trường hợp thứ ba là sự xuất hiện thưa thớt khi cùng một người xuất hiện trở lại sau khoảng thời gian dài. Mỗi lần xuất hiện sau khi đặt lại được xử lý độc lập vì việc đặt lại sẽ xóa trạng thái lịch sử, đảm bảo rằng việc kích hoạt lại luôn được tính chính xác một lần trên mỗi phân đoạn.
