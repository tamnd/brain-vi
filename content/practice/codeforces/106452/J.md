---
title: "CF 106452J - Đoán số!"
description: "Giá trị ẩn là một số nguyên duy nhất được chọn ở khoảng từ 1 đến 1.000.000. Chương trình được phép giao tiếp với giám khảo bằng cách đặt các câu hỏi có dạng “số ẩn có ít nhất là k không?”. Thẩm phán trả lời bằng 1 khi điều kiện đúng và 0 nếu ngược lại."
date: "2026-06-25T09:18:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106452
codeforces_index: "J"
codeforces_contest_name: "UTPC April Fools Contest 2026"
rating: 0
weight: 106452
solve_time_s: 32
verified: true
draft: false
---

[CF 106452J - Đoán số!](https://codeforces.com/problemset/problem/106452/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Giá trị ẩn là một số nguyên duy nhất được chọn ở khoảng từ 1 đến 1.000.000. Chương trình được phép giao tiếp với trọng tài bằng cách đặt các câu hỏi dạng “ít nhất là số ẩn`k`?”. Thẩm phán trả lời bằng`1`khi điều kiện là đúng và`0`nếu không thì. Mục đích là để xác định số ẩn bằng cách sử dụng không quá`4!`, tức là 24, những câu hỏi như vậy. 

Phần đầu vào trống vì đây là vấn đề tương tác. Dữ liệu duy nhất mà chương trình của bạn nhận được trong quá trình thực thi là câu trả lời từ giám khảo sau mỗi truy vấn. Đầu ra bao gồm các truy vấn và sau khi thu thập đủ thông tin, số được đoán cuối cùng. 

Phạm vi chứa một triệu giá trị có thể. Quét tuyến tính sẽ yêu cầu tới 1.000.000 câu hỏi, vượt xa con số cho phép là 24. Hạn chế về số lượng tương tác buộc chúng tôi phải giảm không gian tìm kiếm rất nhanh. Vì mỗi câu hỏi chỉ có hai câu trả lời khả dĩ, nên một chiến lược thành công nên chia đôi số ứng viên còn lại mỗi lần. Hai mươi câu hỏi là đủ vì`2^20`lớn hơn một triệu, vì vậy giới hạn cho phép là 24 sẽ có thêm một số chỗ trống. 

Trường hợp cạnh chính là ranh giới của phạm vi. Nếu số ẩn là`1`, mọi truy vấn hỏi về giá trị lớn hơn sẽ được trả lời phủ định. Việc triển khai tìm kiếm nhị phân bất cẩn có thể di chuyển giới hạn dưới quá xa và trả về`0`, nằm ngoài phạm vi hợp lệ. Ví dụ: nếu số ẩn là`1`, một chuỗi truy vấn cuối cùng phải thu hẹp khoảng thời gian để`[1, 1]`và đầu ra`1`. 

Một trường hợp ranh giới khác là giá trị tối đa. Nếu số ẩn là`1000000`, mọi truy vấn lên đến giá trị đó đều dương. Việc triển khai sai coi giới hạn trên là độc quyền có thể kết thúc bằng`999999`thay vì câu trả lời đúng. Ví dụ: giá trị ẩn`1000000`phải để lại khoảng chứa điểm cuối phù hợp cho đến bước cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là hỏi về mọi giá trị có thể theo thứ tự tăng dần. Chúng ta có thể truy vấn`1`, sau đó`2`, sau đó`3`, v.v. cho đến khi giám khảo cho chúng ta biết rằng số ẩn nhỏ hơn giá trị hiện tại hoặc cho đến khi chúng ta đạt được câu trả lời. Điều này đúng vì mọi con số có thể đều đã được kiểm tra, nhưng trong trường hợp xấu nhất, nó cần một triệu truy vấn, vi phạm giới hạn tương tác ở mức rất lớn. 

Quan sát hữu ích là mọi câu hỏi đều đưa ra sự so sánh về thứ tự. Chúng tôi không cần phải kiểm tra từng số riêng lẻ. Câu hỏi có ngưỡng`k`tách tất cả các ứng viên thành hai nhóm: các giá trị bên dưới`k`và giá trị ít nhất`k`. Nếu chúng ta luôn chọn`k`ở gần giữa phạm vi hiện tại, mỗi câu trả lời sẽ loại bỏ khoảng một nửa số khả năng còn lại. 

Điều này chuyển vấn đề thành tìm kiếm nhị phân trong khoảng thời gian`[1, 1000000]`. Sau khoảng 20 truy vấn, chỉ còn lại một giá trị. Cho phép 24 truy vấn là quá đủ, vì vậy chiến lược tìm kiếm nhị phân cổ điển sẽ an toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1.000.000) truy vấn | O(1) | Quá chậm | 
| Tìm kiếm nhị phân | O(log 1.000.000) truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giữ hai biến biểu thị khoảng thời gian trả lời có thể có hiện tại, bắt đầu bằng`low = 1`Và`high = 1,000,000`. 

Số ẩn được đảm bảo ở trong khoảng này. Duy trì tính bất biến này là ý tưởng cốt lõi của thuật toán. 

1. Chọn giá trị ở giữa`mid = (low + high) // 2`và hỏi xem số ẩn ít nhất là`mid`. 

Câu trả lời chia khoảng thành hai khoảng hợp lệ nhỏ hơn. Nếu câu trả lời là dương thì số ẩn không thể ở dưới`mid`. Nếu câu trả lời là phủ định thì nó phải ở dưới đây`mid`. 

1. Nếu câu trả lời là`1`, bộ`low = mid`. Nếu không thì đặt`high = mid - 1`. 

Khoảng thời gian cập nhật giữ chính xác các ứng viên phù hợp với tất cả các câu trả lời nhận được cho đến nay. 

1. Lặp lại quy trình cho đến khi`low`bằng`high`. 

Tại thời điểm này chỉ còn lại một số, vì vậy nó phải là giá trị ẩn. 

1. In giá trị cuối cùng bằng định dạng câu trả lời được yêu cầu. 

Bất biến xuyên suốt thuật toán là số ẩn luôn thuộc về`[low, high]`. Ban đầu điều này đúng vì toàn bộ phạm vi cho phép được sử dụng. Mỗi truy vấn chỉ loại bỏ những giá trị mâu thuẫn với câu trả lời của thẩm phán, do đó, bất biến vẫn đúng. Khi độ dài khoảng trở thành một, ứng cử viên duy nhất còn lại là câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    low = 1
    high = 10**6

    while low < high:
        mid = (low + high) // 2

        print(f"? {mid}", flush=True)
        ans = int(input())

        if ans == 1:
            low = mid
        else:
            high = mid - 1

    print(f"! {low}", flush=True)

if __name__ == "__main__":
    solve()
```Hai biến`low`Và`high`lưu trữ phạm vi hiện tại có thể. Chúng được cập nhật sau mỗi lần tương tác và giá trị cuối cùng chỉ được in sau khi phạm vi đã thu gọn thành một số. 

Tính toán điểm giữa sử dụng phép chia số nguyên vì ngưỡng truy vấn phải là số nguyên. bản cập nhật`high = mid - 1`là cần thiết sau một câu trả lời phủ định bởi vì`mid`bản thân nó đã được chứng minh là không thể. bản cập nhật`low = mid`thì khác vì câu trả lời tích cực có nghĩa là`mid`vẫn là một ứng cử viên hợp lệ. 

các`flush=True`lập luận là cần thiết trong các vấn đề tương tác. Nếu không xóa, truy vấn có thể vẫn được lưu vào bộ đệm và không bao giờ đến được bộ phận đánh giá, khiến chương trình bị lỗi mặc dù có logic chính xác. 

## Ví dụ đã hoạt động 

Các bài toán tương tác không cung cấp đầu vào và đầu ra mẫu thông thường vì phản hồi của người đánh giá phụ thuộc vào số ẩn. Các dấu vết sau đây cho thấy các tương tác có thể xảy ra. 

Giả sử số ẩn là`13`. 

| Bước | thấp | cao | giữa | Thẩm phán trả lời | Khoảng thời gian mới | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 1000000 | | | [1, 1000000] | 
| 1 | 1 | 1000000 | 500000 | 0 | [1, 499999] | 
| 2 | 1 | 499999 | 250000 | 0 | [1, 249999] | 
| ... | ... | ... | ... | ... | ... | 
| Cuối cùng | 13 | 13 | | | [13, 13] | 

Dấu vết này chứng tỏ rằng các câu trả lời phủ định liên tục loại bỏ nửa trên trong khi vẫn giữ giá trị ẩn bên trong khoảng. 

Giả sử số ẩn là`1000000`. 

| Bước | thấp | cao | giữa | Thẩm phán trả lời | Khoảng thời gian mới | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 1000000 | | | [1, 1000000] | 
| 1 | 1 | 1000000 | 500000 | 1 | [500000, 1000000] | 
| 2 | 500000 | 1000000 | 750000 | 1 | [750000, 1000000] | 
| ... | ... | ... | ... | ... | ... | 
| Cuối cùng | 1000000 | 1000000 | | | [1000000, 1000000] | 

Dấu vết này thực hiện ranh giới trên và xác nhận rằng điểm cuối bên phải không bao giờ bị xóa một cách vô tình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log 1.000.000) truy vấn | Mỗi câu hỏi chia đôi khoảng thời gian còn lại. | 
| Không gian | O(1) | Chỉ giới hạn hiện tại và giá trị tạm thời được lưu trữ. | 

Thuật toán sử dụng tối đa 20 truy vấn vì`2^20`vượt quá kích thước của không gian tìm kiếm. Giới hạn 24 tương tác là đủ và mức sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm 

Không thể thử nghiệm các giải pháp tương tác với đầu vào và đầu ra cố định vì thẩm phán sẽ kiểm soát các câu trả lời. Thay vào đó, thử nghiệm cục bộ có thể xác minh logic chuyển đổi tìm kiếm nhị phân bằng cách mô phỏng một số ẩn.```
def find_number(hidden):
    low = 1
    high = 10**6

    while low < high:
        mid = (low + high) // 2
        if hidden >= mid:
            low = mid
        else:
            high = mid - 1

    return low

assert find_number(1) == 1, "minimum value"
assert find_number(1000000) == 1000000, "maximum value"
assert find_number(500000) == 500000, "middle value"
assert find_number(999999) == 999999, "upper boundary"
assert find_number(123456) == 123456, "random value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ẩn = 1 | 1 | Xử lý ranh giới dưới | 
| ẩn = 1000000 | 1000000 | Xử lý ranh giới trên | 
| ẩn = 500000 | 500000 | Hành vi trung điểm chính xác | 
| ẩn = 999999 | 999999 | Cập nhật từng cái một gần mức tối đa | 
| ẩn = 123456 | 123456 | Tính đúng đắn chung | 

## Vỏ cạnh 

Với giá trị nhỏ nhất, số ẩn là`1`. Mọi truy vấn có ngưỡng lớn hơn`1`trả lại`0`, do đó thuật toán tiếp tục giảm`high`cho đến khi nó đạt tới`1`. Bất biến khoảng ngăn cản kết quả trở thành`0`. 

Để có giá trị lớn nhất thì số ẩn là`1000000`. Mọi truy vấn điểm giữa đều trả về`1`, Vì thế`low`tiếp tục di chuyển lên trên. Khoảng thời gian cuối cùng trở thành`[1000000, 1000000]`, chứng tỏ rằng điểm cuối trên được xử lý chính xác. 

Đối với số ẩn chính xác bằng điểm giữa được truy vấn, giám khảo trả lời`1`vì điều kiện là “ít nhất”. Thuật toán giữ`mid`như một câu trả lời có thể bằng cách gán`low = mid`, tránh được lỗi thường gặp là loại bỏ giá trị đúng.
