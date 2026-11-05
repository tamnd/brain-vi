---
title: "CF 1183D - Hộp Kẹo (bản dễ)"
description: "Chúng tôi được tặng một số bộ sưu tập kẹo độc lập. Mỗi bộ sưu tập chỉ là một tập hợp nhiều số nguyên, trong đó mỗi số nguyên đại diện cho một loại kẹo. Từ mỗi bộ sưu tập, chúng tôi muốn tạo ra một “món quà” duy nhất bằng cách chọn một số loại kẹo."
date: "2026-06-13T11:31:43+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1183
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 570 (Div. 3)"
rating: 1400
weight: 1183
solve_time_s: 142
verified: true
draft: false
---

[CF 1183D - Candy Box (phiên bản dễ)](https://codeforces.com/problemset/problem/1183/D) 

**Đánh giá:** 1400 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một số bộ sưu tập kẹo độc lập. Mỗi bộ sưu tập chỉ là một tập hợp nhiều số nguyên, trong đó mỗi số nguyên đại diện cho một loại kẹo. Từ mỗi bộ sưu tập, chúng tôi muốn tạo ra một “món quà” duy nhất bằng cách chọn một số loại kẹo. 

Hạn chế là số lượng kẹo mỗi loại chúng ta được phép lấy. Nếu chúng ta chọn một số lượng thuộc một loại nhất định thì số tiền đó phải là duy nhất trong số tất cả các loại được chọn. Ví dụ: nếu lấy 3 viên kẹo loại 5 thì không được loại kẹo nào khác trong quà được phép xuất hiện đúng 3 lần. Tuy nhiên, chúng ta có thể lấy 3 thuộc loại 5, 2 thuộc loại 7, 1 thuộc loại 9, v.v., miễn là tất cả các số đếm này đều khác nhau. 

Mục tiêu của mỗi truy vấn là tối đa hóa tổng số kẹo trong quà tặng. 

Kích thước đầu vào lớn trên các truy vấn, tổng số phần tử lên tới 2·10^5. Điều này ngay lập tức loại trừ bất kỳ lý luận bậc hai nào về tần số hoặc mô phỏng tham lam liên tục điều chỉnh số đếm từng cái một cho mỗi loại. Một giải pháp phải xử lý từng truy vấn theo thời gian gần như tuyến tính hoặc tuyến tính. 

Một trường hợp lỗi nhỏ xuất hiện khi nhiều loại có cùng tần số. Ví dụ: nếu chúng ta có tần số [5, 5, 5, 5], một cách tiếp cận đơn giản có thể cố gắng gán tất cả chúng có giá trị 5, nhưng điều đó vi phạm tính duy nhất và chúng ta phải cẩn thận giảm một số số đếm trong khi vẫn duy trì tổng tối đa. 

Một tình huống phức tạp khác là khi tần số nhỏ nhưng nhiều, chẳng hạn như nhiều loại xuất hiện cùng một lúc. Việc chỉ định tất cả chúng giá trị 1 là không hợp lệ vì số đếm phải khác biệt, vì vậy chỉ một loại có thể lấy 1, loại khác phải lấy 0, v.v. 

Những ràng buộc này làm rõ rằng vấn đề không nằm ở việc nhận dạng các loại mà chỉ ở tần số của chúng và cách chúng ta gán các số nguyên dương riêng biệt cho một tập hợp con của chúng để tối đa hóa tổng. 

## Phương pháp tiếp cận 

Ý tưởng tự nhiên đầu tiên là đếm xem mỗi loại có bao nhiêu viên kẹo. Giả sử chúng ta thu được các tần số f1, f2, ..., fk. Bây giờ chúng ta cần gán cho mỗi loại đã chọn một số nguyên dương x riêng biệt, trong đó x ≤ fi và tối đa hóa tổng của tất cả các x đã chọn. 

Một ý tưởng mạnh mẽ là thử tất cả các tập hợp con của các loại và tất cả các phép gán có giá trị riêng biệt, kiểm tra tính khả thi và theo dõi tổng tối đa. Ngay cả khi chúng ta sửa một tập hợp con, việc gán các giá trị riêng biệt cũng là một vấn đề giống như hoán vị. Số lượng tập hợp con là 2^k và k có thể lớn, do đó, điều này nhanh chóng trở nên không khả thi ngay cả đối với các đầu vào nhỏ. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ thử các giá trị từ tần số lớn nhất trở xuống, tham lam gán các số riêng biệt lớn nhất hiện có nhưng tính toán lại các xung đột. Điều này vẫn có nguy cơ xảy ra hành vi O(k^2) khi nhiều tần số tương tự nhau. 

Quan sát quan trọng là chúng tôi không bao giờ quan tâm loại nào sẽ được tính, chỉ có điều chúng tôi chỉ định các số nguyên giảm nghiêm ngặt hoặc ít nhất là khác biệt. Để tối đa hóa tổng, trước tiên chúng ta nên ưu tiên số lượng được chỉ định lớn hơn, nhưng chúng ta bị giới hạn bởi mỗi tần số. 

Điều này biến bài toán thành: chúng ta có một danh sách các khả năng fi, và chúng ta muốn gán cho mỗi phần tử được chọn một số nguyên xi riêng biệt sao cho 1 ₫ xi ₫ fi, tổng xi cực đại. Chiến lược tối ưu là sắp xếp tần số theo thứ tự giảm dần và gán số nguyên chưa sử dụng lớn nhất có thể nhưng không bao giờ vượt quá tần số. 

Khi lặp qua các tần số được sắp xếp, chúng tôi sẽ duy trì giá trị nguyên có sẵn tiếp theo. Nếu tần số nhỏ hơn hoặc bằng hoặc bằng giá trị hiện có, chúng tôi sẽ giới hạn tần số đó; nếu không chúng tôi giảm nó để duy trì sự khác biệt. 

Điều này có hiệu quả vì bất kỳ phép gán nào vi phạm cấu trúc tham lam được sắp xếp đều có thể được sắp xếp lại để tăng hoặc bảo toàn tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k · k) | O(k) | Quá chậm | 
| Tham lam tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào một truy vấn và giảm nó xuống mức xử lý tần suất.

1. Đếm số lần xuất hiện của từng loại kẹo. 

Chúng tôi xây dựng một mảng tần số hoặc từ điển lưu trữ số lượng kẹo thuộc về mỗi loại. Điều này nén đầu vào thành tối đa n giá trị có ý nghĩa. 
2. Trích xuất tất cả các tần số khác 0 vào danh sách. 

Chỉ có tần số mới quan trọng; bản thân các loại không liên quan sau khi đếm. 
3. Sắp xếp tần số theo thứ tự giảm dần. 

Điều này đảm bảo trước tiên chúng tôi chỉ định các giá trị có thể sử dụng lớn hơn, giúp tránh lãng phí dung lượng cho các nhiệm vụ nhỏ. 
4. Khởi tạo một biến`cur = infinity`(hoặc một số lượng rất lớn). 

Điều này thể hiện số lượng khác biệt lớn nhất được phép mà chúng tôi vẫn có thể chỉ định. 
5. Lặp lại từng tần số`f`theo thứ tự sắp xếp. 

Chúng tôi chỉ định`use = min(f, cur - 1)`bởi vì chúng ta phải giữ giá trị giảm dần theo các loại. 

Nếu như`use`trở thành âm hoặc bằng 0, chúng tôi dừng lại, vì không còn phép gán phân biệt dương hợp lệ nào. 
6. Thêm`use`để trả lời và cập nhật`cur = use`. 

Mỗi bước đảm bảo chúng tôi luôn chọn số lượng khác biệt hợp lệ lớn nhất có thể cho loại hiện tại trong khi vẫn tôn trọng các phép gán trước đó. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi xử lý i tần số, chúng tôi đã gán các giá trị giảm dần cho các loại đã chọn và ở mỗi bước, chúng tôi không bao giờ để lại phép gán khả thi không được sử dụng. Việc sắp xếp đảm bảo rằng trước tiên chúng tôi xem xét các loại có dung lượng cao hơn và việc giảm tham lam đảm bảo rằng nếu tần số quá lớn để vừa với các “khe” còn lại, thì chúng tôi sẽ nén tần số đó ở mức tối thiểu. Bất kỳ sai lệch nào so với lựa chọn này sẽ vi phạm tính duy nhất hoặc thay thế phép gán khả thi lớn hơn bằng phép gán nhỏ hơn, làm giảm tổng số tiền. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n = int(input())
        arr = list(map(int, input().split()))

        freq = {}
        for x in arr:
            freq[x] = freq.get(x, 0) + 1

        vals = sorted(freq.values(), reverse=True)

        cur = n
        ans = 0

        for f in vals:
            if cur <= 0:
                break
            use = min(f, cur)
            ans += use
            cur = use - 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén mảng thành tần số bằng từ điển. Điều này là cần thiết vì bản thân các loại không liên quan sau khi đếm. 

Chúng tôi sắp xếp tần số theo thứ tự giảm dần để các nhóm lớn hơn được chỉ định trước, tối đa hóa cơ hội sử dụng sớm số lượng khác biệt lớn. 

Biến`cur`theo dõi số nguyên lớn nhất còn lại mà chúng ta được phép gán. Mỗi lần chúng tôi phân công`use = min(f, cur)`, chúng tôi đảm bảo không vượt quá tần số có sẵn hoặc giá trị khác biệt được phép còn lại. Sau đó chúng tôi giảm`cur`ĐẾN`use - 1`để duy trì tính duy nhất chặt chẽ. 

Một lỗi triển khai phổ biến là quên rằng các giá trị được gán phải hoàn toàn khác biệt. Điều này được thực thi hoàn toàn thông qua`cur - 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 8
a = [1, 4, 8, 4, 5, 6, 3, 8]
```Tần số: 

| Loại | Tần số | 
| --- | --- | 
| 1 | 1 | 
| 3 | 1 | 
| 4 | 2 | 
| 5 | 1 | 
| 6 | 1 | 
| 8 | 2 | 

Tần số được sắp xếp: [2, 2, 1, 1, 1, 1] 

Chúng tôi mô phỏng: 

| Bước | f | cur | sử dụng | trả lời | tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 8 | 2 | 2 | 1 | 
| 2 | 2 | 1 | 1 | 3 | 0 | 
| 3 | 1 | 0 | dừng lại | 3 | - | 

Câu trả lời cuối cùng là 3. 

Điều này cho thấy các tần số lớn bị cắt bớt sớm như thế nào vì tính duy nhất buộc phải thu hẹp nhanh chóng. 

### Ví dụ 2 

đầu vào:```
n = 9
a = [2, 2, 4, 4, 4, 7, 7, 7, 7]
```Tần số: 

[4, 3, 2] 

| Bước | f | cur | sử dụng | trả lời | tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 9 | 4 | 4 | 3 | 
| 2 | 3 | 3 | 3 | 7 | 2 | 
| 3 | 2 | 2 | 2 | 9 | 1 | 

Câu trả lời cuối cùng là 9. 

Điều này chứng tỏ cách lựa chọn tham lam sử dụng đầy đủ từng tần số cho đến khi bị hạn chế bởi tính duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tần suất sắp xếp chiếm ưu thế trong mỗi truy vấn | 
| Không gian | O(n) | Bản đồ tần số và danh sách đếm | 

Tổng của n trên tất cả các truy vấn là 2·10^5, do đó việc sắp xếp trong mỗi truy vấn nhìn chung vẫn hiệu quả. Quét tham lam là tuyến tính về số lượng các loại riêng biệt. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 3 | giảm ràng buộc phân công | 
| tất cả đều khác biệt | 5 | mỗi tần số là 1 | 
| loại đơn | 3 | cắt ngắn dài hạn | 
| hỗn hợp | 4 | tương tác của các tần số khác nhau | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều loại có tần số 1. Đối với đầu vào như [1, 2, 3, 4, 5], thuật toán gán 1 cho loại đầu tiên, sau đó phải gán 0 cho tất cả các loại còn lại. Quá trình giảm tham lam xử lý việc này một cách tự nhiên vì cur nhanh chóng giảm xuống 0. 

Một trường hợp khác là khi một loại chiếm ưu thế, chẳng hạn như [1,1,1,1,1,1]. Nhiệm vụ đầu tiên sử dụng 6, rồi 5, rồi 4, tạo ra một cầu thang cho đến khi kiệt sức. Bất biến đảm bảo chúng ta không bao giờ gán cùng một số đếm hai lần. 

Trường hợp thứ ba là tần số trung bình xen kẽ, chẳng hạn như [3,3,3,2,2]. Việc sắp xếp đảm bảo các nhóm lớn hơn được xử lý trước, ngăn chặn việc chặn sớm các nhiệm vụ cao hơn có thể thực hiện được.
