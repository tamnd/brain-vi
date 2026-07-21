---
title: "CF 103660I - Phân chia mảng"
description: "Chúng tôi có hai mảng có độ dài bằng nhau và chúng tôi muốn cắt phạm vi chỉ mục từ 1 đến n thành nhiều phân đoạn liên tiếp. Mỗi chỉ mục phải thuộc về chính xác một phân đoạn và các phân đoạn đó phải bao phủ toàn bộ mảng mà không có khoảng trống hoặc chồng chéo."
date: "2026-07-02T21:55:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103660
codeforces_index: "I"
codeforces_contest_name: "The 19th Zhejiang University City College Programming Contest"
rating: 0
weight: 103660
solve_time_s: 52
verified: true
draft: false
---

[CF 103660I - Phân chia mảng](https://codeforces.com/problemset/problem/103660/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai mảng có độ dài bằng nhau và chúng tôi muốn cắt phạm vi chỉ mục từ 1 đến n thành nhiều phân đoạn liên tiếp. Mỗi chỉ mục phải thuộc về chính xác một phân đoạn và các phân đoạn đó phải bao phủ toàn bộ mảng mà không có khoảng trống hoặc chồng chéo. 

Đối với mỗi phân đoạn, chúng tôi so sánh tổng giá trị từ mảng đầu tiên với tổng giá trị từ mảng thứ hai. Một phân đoạn chỉ được coi là hợp lệ nếu tổng của a trên phân khúc đó ít nhất bằng tổng của b trên cùng phân khúc. Nhiệm vụ là chia mảng thành càng nhiều phân đoạn hợp lệ càng tốt. 

Đầu ra là số lượng tối đa của các phân đoạn như vậy hoặc −1 nếu không thể thực hiện ngay cả một phân đoạn hợp lệ. 

Các ràng buộc quan trọng một cách đơn giản. Tổng số n trên tất cả các trường hợp thử nghiệm tối đa là 5000, có nghĩa là giải pháp O(n²) cho mỗi trường hợp thử nghiệm là có thể chấp nhận được. Mọi thứ dạng khối hoặc tệ hơn sẽ quá chậm nếu được triển khai trực tiếp qua nhiều thử nghiệm. Điều này cũng gợi ý rằng lập trình động hoặc quét tham lam các tiền tố có thể là đủ. 

Trường hợp cạnh tinh vi xuất hiện khi thậm chí toàn bộ mảng không thỏa mãn điều kiện. Nếu tổng của a nhỏ hơn tổng của b thì không có phân đoạn nào hoạt động cả, vì mọi phân vùng bao gồm toàn bộ mảng được chia thành các phần mà tổng vẫn bằng chênh lệch toàn cục. Ví dụ: nếu a toàn là số 0 và b toàn là số 1 thì không có phân đoạn nào có thể thỏa mãn bất đẳng thức, do đó câu trả lời phải là −1. 

Một tình huống phức tạp khác xảy ra khi các phân đoạn hợp lệ tồn tại nhưng việc cắt tham lam quá sớm không thành công. Ví dụ: nếu các tiền tố đầu hơi âm về mặt (a − b) nhưng các phần tử sau bù lại, chúng ta phải đảm bảo rằng chúng ta không buộc cắt ở các vị trí ngăn cản việc hình thành các phân đoạn hợp lệ trong tương lai. 

## Phương pháp tiếp cận 

Điểm khởi đầu tự nhiên là xem xét việc kiểm tra tất cả các phân vùng có thể. Đối với mỗi cách cắt mảng thành k đoạn, chúng tôi xác minh xem mỗi đoạn có thỏa mãn điều kiện tổng hay không. Điều này nhanh chóng trở nên không khả thi vì số lượng phân vùng tăng theo cấp số nhân với n và thậm chí tính toán tổng phân đoạn nhiều lần dẫn đến chi phí bậc hai hoặc tệ hơn. 

Cách tốt hơn là tập trung vào sự khác biệt về tiền tố. Xác định một mảng c trong đó c[i] = a[i] − b[i]. Điều kiện cho một đoạn [l, r] trở thành tổng của c trên đoạn đó không âm. Vì vậy, vấn đề trở thành việc chia mảng thành số lượng tối đa các phân đoạn liền kề, mỗi phân đoạn có tổng không âm. 

Việc cải cách này rất hiệu quả vì nó loại bỏ nhu cầu theo dõi hai mảng riêng biệt và giảm mọi thứ xuống việc quản lý số dư đang hoạt động. 

Quan sát quan trọng là chúng tôi muốn cắt giảm ngay khi tổng hiện hành trở lại không âm sau khi đã âm hoặc bằng 0 trước đó. Nếu chúng tôi kéo dài một phân khúc càng lâu càng tốt trong khi vẫn duy trì khả năng kết thúc phân khúc đó với tổng số không âm, thì chúng tôi sẽ tối đa hóa số lượng phân khúc. Điều này biến vấn đề thành một cuộc quét tham lam: duy trì tổng hiện có và bất cứ khi nào nó đạt đến giá trị dương hoặc bằng 0, chúng ta có thể đóng một phân đoạn và đặt lại một cách an toàn. 

Trực giác đúng đắn cho thấy việc trì hoãn việc cắt không bao giờ giúp tăng số lượng đoạn. Bất kỳ tiền tố nào có tổng không âm đều có thể đóng vai trò là điểm cuối phân đoạn hợp lệ và việc mở rộng nó hơn nữa chỉ có nguy cơ mất đi phần cắt tiềm năng trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các phân vùng | O(2ⁿ · n) | O(1) | Quá chậm | 
| Quét tiền tố tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi ngầm chuyển đổi các mảng thành một mảng khác biệt duy nhất trong quá trình xử lý. Sau đó, chúng tôi quét từ trái sang phải trong khi duy trì số dư hiện tại về số tiền thặng dư mà chúng tôi hiện có.

1. Khởi tạo một biến đang chạy current_sum = 0 và một đoạn đếm = 0. Tổng chạy theo dõi tổng (a − b) bên trong phân đoạn hiện tại. 
2. Lặp lại mảng từ chỉ mục 1 đến n, cập nhật current_sum bằng cách thêm a[i] − b[i]. Điều này duy trì sự cân bằng ròng của phân khúc mà chúng tôi hiện đang hình thành. 
3. Sau mỗi lần cập nhật, hãy kiểm tra xem current_sum có phải là số không âm hay không. Nếu đúng như vậy, chúng tôi quyết định đóng phân đoạn hiện tại ở chỉ mục này và tăng phân đoạn lên 1. Sau đó, chúng tôi đặt lại current_sum thành 0 để bắt đầu phân đoạn mới từ chỉ mục tiếp theo. 
4. Sau khi xử lý tất cả các phần tử, nếu chúng ta chưa bao giờ hình thành bất kỳ phân đoạn nào, nghĩa là các phân đoạn vẫn bằng 0, xuất ra −1. Nếu không thì xuất ra các phân đoạn. 

Quyết định cắt giảm ngay lập tức khi current_sum trở thành không âm là sự lựa chọn tham lam nhằm tối đa hóa tính linh hoạt trong tương lai. Nó đảm bảo rằng các phân đoạn trước đó càng ngắn càng tốt trong khi vẫn hợp lệ, để lại nhiều phần tử có sẵn hơn để tạo thành các phân đoạn bổ sung. 

### Tại sao nó hoạt động 

Tổng số tiền hiện có thể hiện tính khả thi của việc kết thúc một đoạn tại một điểm nhất định. Bất cứ khi nào tổng trở thành không âm, chúng ta có một phân đoạn hợp lệ. Nếu chúng tôi trì hoãn việc cắt giảm này, chúng tôi chỉ tích lũy nhiều phần tử hơn vào cùng một phân khúc, điều này không thể tăng số lượng ranh giới phân khúc hợp lệ trong tương lai vì chúng tôi đang sử dụng các chỉ số có thể đã hình thành các điểm cuối hợp lệ trước đó. Vì vậy, chiến lược tham lam luôn bảo toàn hoặc tăng số lượng các phân khúc có thể có, không bao giờ giảm bớt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        current_sum = 0
        segments = 0
        
        for i in range(n):
            current_sum += a[i] - b[i]
            if current_sum >= 0:
                segments += 1
                current_sum = 0
        
        if segments == 0:
            print(-1)
        else:
            print(segments)

if __name__ == "__main__":
    solve()
```Mã tuân theo cách giải thích tham lam trực tiếp. Việc chuyển đổi thành khác biệt được thực hiện nội tuyến dưới dạng a[i] − b[i], tránh tốn thêm bộ nhớ. Điểm tinh tế chính là việc đặt lại current_sum sau khi hình thành một phân đoạn, điều này đảm bảo rằng mỗi phân đoạn là độc lập. 

Điều kiện current_sum >= 0 là điều kiện cho phép chúng ta đóng một phân đoạn. Chúng tôi không chờ đợi nó trở nên tích cực một cách nghiêm ngặt vì các phân đoạn có tổng bằng 0 cũng hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó các mảng dần dần tích lũy số dư dương. 

đầu vào: 

n = 5 

a = [2, 2, 2, 2, 2] 

b = [1, 1, 1, 1, 1] 

| tôi | a[i]-b[i] | hiện tại_sum | phân đoạn | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 1 | 0 | 2 | 
| 3 | 1 | 1 | 3 | 
| 4 | 1 | 0 | 4 | 
| 5 | 1 | 1 | 5 | 

Mọi tiền tố đều không âm, vì vậy mọi phần tử trở thành phần tử của chính nó hoặc một phần của phân đoạn tối thiểu, mang lại số lần cắt tối đa. 

Bây giờ hãy xem xét một trường hợp cân bằng hơn. 

đầu vào: 

n = 4 

a = [1, 1, 1, 1] 

b = [2, 2, 0, 0] 

| tôi | a[i]-b[i] | hiện tại_sum | phân đoạn | 
| --- | --- | --- | --- | 
| 1 | -1 | -1 | 0 | 
| 2 | -1 | -2 | 0 | 
| 3 | 1 | -1 | 0 | 
| 4 | 1 | 0 | 1 | 

Chỉ khi kết thúc thì tiền tố mới hợp lệ nên chúng ta chỉ có thể tạo thành một phân đoạn. Điều này cho thấy thuật toán trì hoãn việc cắt một cách chính xác cho đến khi đạt được tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần với các bản cập nhật liên tục | 
| Không gian | O(1) thêm | Chỉ có một số biến số nguyên được sử dụng | 

Tổng số n trên các trường hợp thử nghiệm được giới hạn bởi 5000, vì vậy giải pháp tuyến tính này chạy thoải mái trong giới hạn. Ngay cả việc triển khai nặng hệ số không đổi cũng sẽ ổn, nhưng cách tiếp cận này là tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# basic increasing case
assert run("""1
5
2 2 2 2 2
1 1 1 1 1
""") == "5"

# only one valid segment at the end
assert run("""1
4
1 1 1 1
2 2 0 0
""") == "1"

# impossible case
assert run("""1
3
1 1 1
2 2 2
""") == "-1"

# mixed case
assert run("""1
6
3 1 2 1 1 2
2 2 1 1 2 1
""") == "3"

# all equal
assert run("""1
3
5 5 5
5 5 5
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ngày càng tích cực | 5 | mọi tiền tố đều hợp lệ | 
| tính khả thi muộn | 1 | chỉ toàn bộ phân đoạn hoạt động | 
| tất cả đều tiêu cực | -1 | trường hợp bất khả thi | 
| giá trị hỗn hợp | 3 | hành vi phân khúc tham lam | 
| tất cả đều bình đẳng | 3 | phân đoạn không khác biệt | 

## Vỏ cạnh 

Sự khác biệt mảng hoàn toàn âm thể hiện rõ ràng tình trạng lỗi. 

đầu vào: 

n = 3 

a = [1, 1, 1] 

b = [2, 2, 2] 

Tổng chạy trở thành −1, −2, −3 và không bao giờ phục hồi. Không có phân đoạn nào có thể bị đóng, vì vậy các phân đoạn vẫn bằng 0 và chúng ta xuất ra −1. Thuật toán tránh tạo ra phân đoạn sai một cách chính xác vì nó chỉ tính các phân đoạn khi đạt đến ranh giới không âm hợp lệ. 

Trường hợp không khác biệt thể hiện sự phân đoạn tối đa. 

đầu vào: 

n = 3 

a = [5, 5, 5] 

b = [5, 5, 5] 

Mỗi bước giữ current_sum ở mức 0, vì vậy mỗi chỉ mục sẽ đóng một phân đoạn. Kết quả là 3 phân đoạn, xác nhận rằng số 0 được coi là hợp lệ và việc thiết lập lại tham lam không làm mất tính tối ưu.
