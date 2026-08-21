---
title: "CF 104081A - \u51cf\u80a5\u8ba1\u5212"
description: "Chúng ta được xếp một hàng người, mỗi người có trọng lượng cố định và một trò chơi liên tục so sánh những người ở đầu hàng. Ở mỗi vòng, hai người đầu tiên xếp hàng sẽ thi đấu. Người nào nặng hơn sẽ thắng vòng đó."
date: "2026-07-02T02:35:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104081
codeforces_index: "A"
codeforces_contest_name: "2022\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 104081
solve_time_s: 58
verified: true
draft: false
---

[CF 104081A - \u51cf\u80a5\u8ba1\u5212](https://codeforces.com/problemset/problem/104081/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp một hàng người, mỗi người có trọng lượng cố định và một trò chơi liên tục so sánh những người ở đầu hàng. Ở mỗi vòng, hai người đầu tiên xếp hàng sẽ thi đấu. Người nào nặng hơn sẽ thắng vòng đó. Người chiến thắng ở lại phía trước và tiếp tục thi đấu trong các vòng sau, trong khi người thua cuộc sẽ bị đưa ra phía sau hàng đợi. Chúng tôi cũng theo dõi các chiến thắng liên tiếp: mỗi khi ai đó thắng một vòng, chuỗi trận hiện tại của họ sẽ tăng lên và thời điểm bất kỳ người tham gia nào đạt được số trận thắng liên tiếp cần thiết, trò chơi sẽ kết thúc ngay lập tức và người tham gia đó được tuyên bố là người chiến thắng chung cuộc. 

Đầu vào đưa ra thứ tự ban đầu của hàng đợi và số trận thắng liên tiếp cần thiết để kết thúc trò chơi. Đầu ra là vị trí của người chiến thắng cuối cùng theo thứ tự ban đầu. 

Hạn chế chính của giải pháp này là hàng đợi có thể phát triển với số vòng rất lớn nếu được mô phỏng trực tiếp. Mỗi vòng là O(1), nhưng trong trường hợp xấu nhất, chúng ta có thể thực hiện một số lượng lớn các hoán đổi và so sánh trước khi xác định được người chiến thắng, đặc biệt nếu các ứng cử viên mạnh bắt đầu ở khoảng cách xa. Điều này loại trừ bất kỳ cách tiếp cận nào mô phỏng nhiều lần toàn bộ quá trình mà không có cái nhìn sâu sắc về hành vi lâu dài của nó. 

Một trường hợp phức tạp xuất phát từ định nghĩa về những chiến thắng liên tiếp. Nếu người ta hiểu quy trình là cần mô phỏng rõ ràng việc đặt lại liên tục cho tất cả người tham gia thì trạng thái sẽ trở nên phức tạp quá mức. Một cạm bẫy khác là giả định người chiến thắng có thể phụ thuộc vào các tương tác ngẫu nhiên sớm trong hàng đợi; trên thực tế, quá trình này có yếu tố quyết định chi phối. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp tuân theo các quy tắc theo đúng nghĩa đen. Chúng tôi duy trì một hàng đợi, liên tục bật hai phần tử đầu tiên, so sánh trọng số của chúng và đẩy phần tử thua về phía sau. Chúng tôi cũng duy trì một bản đồ về chuỗi chiến thắng hiện tại. Bất cứ khi nào ai đó thắng, chuỗi trận của họ sẽ tăng lên và chuỗi trận của đối thủ sẽ được đặt lại. Ngay khi bất kỳ chuỗi nào đạt đến ngưỡng yêu cầu, chúng tôi sẽ dừng lại. 

Cách tiếp cận này đúng vì nó phản ánh chính xác quá trình. Tuy nhiên, nó có thể xuống cấp trầm trọng. Nếu người mạnh nhất bắt đầu ở gần phía sau, họ có thể phải vượt qua nhiều đối thủ trước khi đến được phía trước. Mỗi tương tác có thời gian không đổi, nhưng tổng số tương tác có thể tăng rất lớn, khiến phương pháp này có khả năng quá chậm đối với đầu vào lớn. 

Quan sát quan trọng là quá trình này bị chi phối bởi trọng số tối đa toàn cầu. Bất kỳ người nào yếu hơn mức tối đa không bao giờ có thể đánh bại nó và một khi mức tối đa đến trước hàng đợi, nó sẽ thắng ở mọi vòng tiếp theo. Kể từ thời điểm đó, chuỗi chiến thắng của nó tăng lên một cách rõ ràng cho đến khi đạt đến ngưỡng yêu cầu. Do đó, toàn bộ quá trình chỉ phụ thuộc vào thời điểm phần tử tối đa xuất hiện ở phía trước chứ không phụ thuộc vào chuỗi so sánh trung gian đầy đủ. 

Điều này làm giảm vấn đề tìm chỉ mục của phần tử lớn nhất trong mảng ban đầu. Yếu tố đó đảm bảo cuối cùng sẽ tích lũy được số trận thắng liên tiếp cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng đầy đủ | O(n + hoạt động) có khả năng rất lớn | O(n) | Quá chậm | 
| Quan sát phần tử tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giải quyết vấn đề bằng cách xác định yếu tố chắc chắn chiếm ưu thế trong mọi so sánh. 

1. Quét qua danh sách trọng số trong khi theo dõi giá trị lớn nhất và vị trí của nó trong mảng ban đầu. Vị trí là những gì cuối cùng chúng ta cần xuất ra. 
2. Mỗi khi chúng tôi thấy một giá trị lớn hơn mức tối đa hiện tại, chúng tôi sẽ cập nhật cả giá trị tối đa và chỉ mục của nó. Điều này đảm bảo chúng tôi luôn ghi nhớ người tham gia mạnh nhất toàn cầu. 
3. Sau khi quá trình quét kết thúc, chúng tôi trả về chỉ mục được lưu trữ của phần tử tối đa làm câu trả lời. 

### Tại sao nó hoạt động

Quá trình này chỉ so sánh các phần tử liền kề, nhưng bất kỳ phần tử nào yếu hơn mức tối đa toàn cục sẽ không bao giờ có thể loại bỏ được nó. Một khi phần tử tối đa đạt đến đầu hàng thông qua các vòng quay lặp đi lặp lại, không có phần tử nào khác có thể đánh bại nó, vì vậy nó sẽ tích lũy chiến thắng vô thời hạn. Vì mọi người tham gia khác cuối cùng đều phải thua nó trong chuỗi so sánh trực tiếp, yếu tố đầu tiên đảm bảo chuỗi chiến thắng không bị gián đoạn là mức tối đa toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

max_val = -10**18
max_idx = -1

for i, v in enumerate(a, 1):
    if v > max_val:
        max_val = v
        max_idx = i

print(max_idx)
```Việc triển khai chỉ đơn giản là theo dõi giá trị tối đa trong khi vẫn giữ nguyên vị trí được lập chỉ mục 1 ban đầu. Tham số thứ hai`k`không ảnh hưởng đến kết quả cuối cùng vì phần tử tối đa cuối cùng sẽ đạt được bất kỳ ngưỡng thắng liên tiếp cần thiết nào khi nó chiếm ưu thế ở đầu hàng đợi. 

Một lỗi phổ biến ở đây là cố gắng mô phỏng động lực của hàng đợi một cách rõ ràng. Điều đó dẫn đến sự phức tạp không cần thiết và che khuất sự thật rằng danh tính người chiến thắng đã được cố định ngay từ đầu. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có trọng số`[1, 3, 2, 5, 4]`. Tối đa là`5`, nằm ở vị trí`4`. Bất kể những so sánh trung gian, mọi yếu tố khác cuối cùng sẽ thua`5`và một khi nó hoạt động ở phía trước, nó sẽ tiếp tục giành chiến thắng. 

| Bước | Tối đa hiện tại | Chỉ số tối đa | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 2 | 
| 3 | 3 | 2 | 
| 4 | 5 | 4 | 
| 5 | 5 | 4 | 

Dấu vết này cho thấy rằng chỉ có cực đại toàn cục mới quan trọng và cực đại cục bộ trước đó không ảnh hưởng đến kết quả cuối cùng. 

Một ví dụ khác,`[7, 1, 6, 2]`, mức tối đa đã ở phía trước. Câu trả lời ngay lập tức được lập chỉ mục`1`và không có tương tác nào trong hàng đợi có thể thay đổi điều đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tìm phần tử lớn nhất | 
| Không gian | O(1) | Chỉ sử dụng các biến phụ không đổi | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó thực hiện quét tuyến tính một lần trên đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    mx = -10**18
    idx = -1
    for i, v in enumerate(a, 1):
        if v > mx:
            mx = v
            idx = i
    return str(idx)

# sample-like checks
assert solve("6 3\n1 1 4 5 1 4\n") == "4"

# minimum size
assert solve("1 10\n5\n") == "1"

# already maximum at front
assert solve("4 2\n9 1 2 3\n") == "1"

# maximum at end
assert solve("4 2\n1 2 3 9\n") == "4"

# all equal
assert solve("5 100\n7 7 7 7 7\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp ranh giới tối thiểu | 
| tối đa ở phía trước | 1 | thống trị sớm | 
| tối đa ở cuối | n | tính đúng đắn của việc lập chỉ mục | 
| tất cả đều bình đẳng | 1 | hành vi đứt dây buộc ổn định |
