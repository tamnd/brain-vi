---
title: "CF 103104F - Pin"
description: "Chúng tôi được cấp một bộ sưu tập pin và một bộ sưu tập các địa điểm chụp. Mỗi pin có một lượng năng lượng cố định và mỗi vị trí cần một lượng năng lượng cố định để hoàn thành một phiên ghi."
date: "2026-07-03T21:43:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103104
codeforces_index: "F"
codeforces_contest_name: "2021 Hubei Provincial Collegiate Programming Contest"
rating: 0
weight: 103104
solve_time_s: 49
verified: true
draft: false
---

[CF 103104F - Pin](https://codeforces.com/problemset/problem/103104/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ sưu tập pin và một bộ sưu tập các địa điểm chụp. Mỗi pin có một lượng năng lượng cố định và mỗi vị trí cần một lượng năng lượng cố định để hoàn thành một phiên ghi. Mỗi pin chỉ có thể được sử dụng tối đa một lần và khi bạn bắt đầu sử dụng pin cho một địa điểm, pin đó phải đáp ứng đầy đủ yêu cầu của địa điểm đó mà không bị gián đoạn hoặc thay thế. 

Nhiệm vụ là chọn chiến lược ghép nối giữa pin và vị trí sao cho số lượng vị trí hoàn thành đầy đủ được tối đa hóa. Mỗi pin có thể được gán cho nhiều nhất một vị trí và mỗi vị trí có thể được gán nhiều nhất một pin. Vị trí được coi là hài lòng nếu pin được chọn có dung lượng ít nhất bằng thời lượng yêu cầu. 

Cấu trúc ngay lập tức gợi ý một vấn đề giống như đối sánh hai bên, nhưng các ràng buộc quá lớn đối với bất kỳ thuật toán đối sánh chung nào. Thay vào đó, đây là bài toán gán tham lam trên hai tập hợp. 

Các ràng buộc ngụ ý rằng chúng ta cần một cái gì đó gần với O(N log N + M log M) hoặc O(N + M log M). Với N lên tới 100000 và M lên tới 300000, mọi thứ bậc hai hoặc thậm chí N nhân M đều là không thể. Một giải pháp thử mọi ghép nối vị trí pin rõ ràng là không khả thi vì nó sẽ đạt tới mức so sánh lên tới 3 × 10^10 trong trường hợp xấu nhất. 

Chi tiết cấu trúc quan trọng là tất cả thời lượng chụp đều là lũy thừa của hai. Đây là một gợi ý rõ ràng rằng việc kết hợp tham lam theo nhóm hoặc nhóm sẽ có hiệu quả vì chúng ta có thể xử lý các yêu cầu theo thứ tự tăng dần và xử lý các giá trị lặp lại một cách hiệu quả. 

Một số trường hợp đặc biệt quan trọng: 

Một kẻ tham lam ngây thơ chỉ định viên pin nhỏ nhất có thể đáp ứng một vị trí mà không đặt hàng cẩn thận có thể thất bại. Ví dụ: nếu pin là [5, 6] và yêu cầu là [4, 5], một bài tập bất cẩn có thể cho 5 → 5 và 6 → 4, điều này không sao cả, nhưng nếu chúng ta đảo ngược các quyết định một cách tham lam theo thứ tự sai, chúng ta có thể lãng phí 6 trên 5 và để lại 4 không khớp tùy theo thứ tự thực hiện. Một thất bại tinh tế khác phát sinh khi tồn tại nhiều yêu cầu giống hệt nhau, vì sự lựa chọn tham lam phải tôn trọng sự tối ưu toàn cục hơn là sự phù hợp cục bộ. 

## Phương pháp tiếp cận 

Chế độ xem brute-force sẽ cố gắng phân bổ pin vào các vị trí theo mọi cách có thể và tính toán kết quả phù hợp nhất. Điều này tương ứng với việc thử tất cả các kết quả khớp trong biểu đồ lưỡng cực trong đó tồn tại một cạnh nếu ai ≥ bi. Ngay cả khi chúng tôi hạn chế thực hiện một nhiệm vụ tham lam, mô phỏng ngây thơ sẽ kiểm tra mọi pin ở mọi vị trí, tạo ra độ phức tạp về thời gian O(NM), vượt xa giới hạn. 

Quan sát quan trọng là chúng ta không cần bảo toàn danh tính của các vị trí vượt quá năng lượng cần thiết của chúng. Vì yêu cầu chỉ là kích thước và chúng ta chỉ quan tâm đến việc chúng ta đáp ứng được bao nhiêu, nên bài toán trở thành bài toán cổ điển “tối đa hóa số lượng kết quả phù hợp khi công suất ≥ nhu cầu”. Điều này thường được giải quyết bằng cách sắp xếp cả hai mảng và kết hợp tham lam từ nhỏ nhất đến lớn nhất. 

Tuy nhiên, điều khó hiểu là b_i là lũy thừa của hai. Điều này đảm bảo số lượng giá trị yêu cầu riêng biệt rất hạn chế (nhiều nhất là 31). Thay vì coi M là lớn, chúng ta có thể nén các yêu cầu thành các nhóm tần số được lập chỉ mục theo số mũ. Sau đó, chúng tôi kết hợp pin một cách hiệu quả với các mức nhu cầu từ nhỏ đến lớn. 

Chúng tôi sắp xếp pin theo thứ tự tăng dần và cũng xử lý các nhóm yêu cầu từ công suất nhỏ nhất đến công suất lớn nhất. Đối với mỗi kích thước yêu cầu, chúng tôi cố gắng chỉ định loại pin nhỏ nhất có thể mà vẫn đáp ứng được. Điều này đảm bảo chúng tôi không bao giờ lãng phí pin lớn cho một yêu cầu nhỏ trừ khi cần thiết. 

Chúng ta duy trì con trỏ về pin và tiêu thụ chúng một cách tham lam khi đáp ứng được nhu cầu. Vì cả hai bên đều được sắp xếp và chúng ta chỉ tiến về phía trước nên mỗi phần tử được xử lý một lần.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu | O(NM) | O(1) | Quá chậm | 
| Sắp xếp + So khớp tham lam | O((N + M) log N) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả dung lượng pin và sắp xếp chúng theo thứ tự không giảm. Điều này cho phép chúng tôi luôn sử dụng loại pin nhỏ nhất hiện có mà vẫn có thể đáp ứng được yêu cầu, bảo toàn pin lớn hơn cho những nhu cầu lớn hơn sau này. 
2. Đếm xem có bao nhiêu yêu cầu ứng với lũy thừa của hai giá trị. Vì bi = 2^j nên chúng ta lưu trữ tần số trong một mảng freq[j]. Điều này nén đầu vào thành tối đa 31 danh mục. 
3. Khởi tạo con trỏ i ở đầu mảng pin đã được sắp xếp. Con trỏ này đại diện cho lượng pin nhỏ nhất chưa sử dụng. 
4. Lặp lại các lũy thừa có thể có của hai từ nhỏ nhất đến lớn nhất, xử lý freq[j] theo thứ tự tăng dần của công suất yêu cầu. Điều này đảm bảo chúng ta luôn cố gắng hoàn thành những nhiệm vụ dễ dàng hơn trước tiên. 
5. Đối với mỗi mức yêu cầu 2^j, hãy cố gắng phân bổ pin một cách tham lam. Trong khi freq[j] > 0 và i < N, hãy tăng i cho đến khi tìm được pin có dung lượng ≥ 2^j. Nếu không tồn tại, hãy tạm dừng vì sẽ không thể thực hiện thêm nhiệm vụ nào ở cấp độ này hoặc cao hơn cho phạm vi pin này. 
6. Khi tìm thấy pin hợp lệ, hãy gán pin đó cho một yêu cầu, tăng i, giảm tần số [j] và tăng bộ đếm câu trả lời. Điều này tiêu tốn cả pin và vị trí. 
7. Tiếp tục cho đến khi tất cả các mức yêu cầu được xử lý hoặc hết pin. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến tham lam: tại bất kỳ thời điểm nào, lượng pin nhỏ nhất còn lại hoặc quá nhỏ so với mức yêu cầu hiện tại hoặc là lựa chọn tốt nhất có thể cho mức đó. Vì cả pin và yêu cầu đều được xử lý theo thứ tự tăng dần nên bất kỳ sai lệch nào khi chúng tôi gán pin lớn hơn cho yêu cầu nhỏ hơn sẽ chỉ làm giảm tính linh hoạt cho các yêu cầu lớn hơn trong tương lai mà không làm tăng số lượng nhiệm vụ được đáp ứng. Cấu trúc đơn điệu đảm bảo rằng một khi pin bị bỏ qua cho một yêu cầu, nó sẽ không bao giờ hữu ích cho các yêu cầu nhỏ hơn và một khi yêu cầu tăng lên, các pin trước đó sẽ không thể đáp ứng chúng nữa. Điều này ngăn chặn bất kỳ trao đổi có lợi nào có thể cải thiện tổng số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    a.sort()

    freq = [0] * 31
    for x in b:
        freq[x.bit_length() - 1] += 1

    ans = 0
    i = 0

    for j in range(31):
        need = 1 << j
        while freq[j] > 0 and i < n:
            if a[i] >= need:
                ans += 1
                freq[j] -= 1
                i += 1
            else:
                i += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc phân loại pin để chúng ta luôn có thể tiêu thụ chúng theo thứ tự tăng dần. Dải tần số nén tất cả thời lượng quay vào nhóm công suất hai, điều này rất quan trọng vì nó loại bỏ sự phụ thuộc vào M trong vòng lặp khớp. 

Con trỏ i không bao giờ được đặt lại về phía sau. Điều này là an toàn vì một khi pin quá nhỏ so với mức yêu cầu, nó sẽ không bao giờ hữu ích cho bất kỳ cấp độ nào sau này. Mỗi pin được xem xét tối đa một lần, đảm bảo quét tuyến tính trên mảng đã sắp xếp. 

Vòng lặp bên trong tiêu thụ pin hoặc loại bỏ pin vĩnh viễn, đảm bảo tiến độ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 2, M = 4
a = [3, 5]
b = [2, 2, 2, 2]
```Xô: 

tần số [1] = 4 vì 2 = 2^1 

| Bước | tôi | Pin a[i] | tần số[1] | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 4 | dùng 3 cho 2 | 1 | 
| 2 | 1 | 5 | 3 | sử dụng 5 cho 2 | 2 | 
| 3 | 2 | - | 2 | dừng lại | 2 | 

Chúng tôi khớp hai vị trí vì chỉ tồn tại hai pin. Dấu vết cho thấy một khi pin cạn kiệt thì các yêu cầu còn lại không thể được đáp ứng. 

### Ví dụ 2 

đầu vào:```
N = 3, M = 3
a = [4, 6, 8]
b = [2, 4, 4]
```Xô: 

tần số [1] = 1, tần số [2] = 2 

| Bước | tôi | Pin | Cấp độ | tần số | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 2 | 1 | sử dụng 4 | 1 | 
| 2 | 1 | 6 | 4 | 2 | sử dụng 6 | 2 | 
| 3 | 2 | 8 | 4 | 1 | sử dụng 8 | 3 | 

Điều này xác nhận rằng việc xử lý các yêu cầu nhỏ hơn trước tiên sẽ tránh lãng phí pin lớn hơn một cách không cần thiết, đồng thời vẫn cho phép chúng đáp ứng các nhiệm vụ lớn hơn sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + M + 31) | Việc sắp xếp pin chiếm ưu thế, tập hợp tần số và quét là tuyến tính | 
| Không gian | O(1) thêm | Chỉ mảng tần số có kích thước cố định và lưu trữ đầu vào được sắp xếp | 

Các ràng buộc cho phép tổng số phần tử lên tới 4 × 10^5 và thuật toán chỉ thực hiện sắp xếp cộng với một lần quét tuyến tính duy nhất trên pin, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log2

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    a.sort()
    freq = [0] * 31
    for x in b:
        freq[x.bit_length() - 1] += 1

    ans = 0
    i = 0

    for j in range(31):
        need = 1 << j
        while freq[j] > 0 and i < n:
            if a[i] >= need:
                ans += 1
                freq[j] -= 1
                i += 1
            else:
                i += 1

    return str(ans)

# provided sample
assert run("2 4\n3 5\n2 2 2 2\n") == "2"

# all equal small
assert run("3 3\n1 1 1\n1 1 1\n") == "3"

# insufficient batteries
assert run("2 5\n10 20\n2 2 2 2 2\n") == "2"

# large battery single match
assert run("1 1\n100\n64\n") == "1"

# greedy ordering trap check
assert run("3 3\n3 5 10\n2 4 8\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu bằng nhau | 3 | kết hợp đầy đủ cơ bản | 
| không đủ pin | 2 | xử lý kiệt sức | 
| trận đấu lớn duy nhất | 1 | ngưỡng đúng đắn | 
| đặt hàng hỗn hợp | 3 | tham lam đặt hàng an toàn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả pin đều nhỏ hơn yêu cầu nhỏ nhất. Trong trường hợp đó, con trỏ i di chuyển qua tất cả các pin mà không khớp và kết quả vẫn là 0. Thuật toán bỏ qua một cách chính xác tất cả các pin không sử dụng được vì điều kiện a[i] < cần chỉ kích hoạt sự tiến bộ. 

Một trường hợp khác xảy ra khi có nhiều giá trị yêu cầu giống nhau. Bởi vì chúng tôi xử lý việc đếm tần số nên mỗi kết quả phù hợp sẽ tiêu thụ chính xác một đơn vị nhu cầu và vòng lặp sẽ tiếp tục cho đến khi hết pin hoặc nhu cầu. Không có sự phụ thuộc vào thứ tự trong các yêu cầu giống hệt nhau, do đó không phát sinh hành vi bệnh lý. 

Trường hợp khó khăn cuối cùng là khi tồn tại một số pin rất lớn cùng với nhiều pin nhỏ. Việc phân loại đảm bảo rằng pin nhỏ được sử dụng trước tiên nếu có thể, nhưng khi yêu cầu tăng lên, thuật toán sẽ tự nhiên chuyển sang pin lớn hơn. Vì con trỏ không bao giờ di chuyển lùi nên pin lớn sẽ được bảo toàn tự động cho các yêu cầu lớn hơn.
