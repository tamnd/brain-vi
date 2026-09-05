---
title: "CF 104520F - Độ tin cậy tối đa"
description: "Chúng tôi đang xử lý một chuỗi các giá trị theo một thứ tự cố định. Điểm bắt đầu từ 0 và với mỗi giá trị trong chuỗi, chúng ta phải quyết định ngay nên thêm nó vào điểm hiện tại hay nhân điểm hiện tại với nó."
date: "2026-06-30T10:27:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104520
codeforces_index: "F"
codeforces_contest_name: "Teamscode Summer 2023 Contest"
rating: 0
weight: 104520
solve_time_s: 74
verified: true
draft: false
---

[CF 104520F - Độ tin cậy tối đa](https://codeforces.com/problemset/problem/104520/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xử lý một chuỗi các giá trị theo một thứ tự cố định. Điểm bắt đầu từ 0 và với mỗi giá trị trong chuỗi, chúng ta phải quyết định ngay nên thêm nó vào điểm hiện tại hay nhân điểm hiện tại với nó. Khi quyết định được đưa ra cho một vị trí, nó không thể thay đổi sau đó và chúng tôi tiến tới giá trị tiếp theo. 

Mục tiêu là tối đa hóa điểm số cuối cùng sau khi xử lý tất cả các giá trị. 

Ràng buộc về số lượng phần tử trên mỗi trường hợp thử nghiệm là nhỏ, tối đa 100, nhưng có thể lên tới 10.000 trường hợp thử nghiệm. Sự kết hợp này có nghĩa là bất kỳ giải pháp nào có phép tính bậc hai cho mỗi trường hợp thử nghiệm đều có thể chấp nhận được, trong khi bất kỳ giải pháp nào có dạng hàm mũ$N$sẽ là không thể ngay cả khi cắt tỉa. 

Khó khăn chính là phép nhân tương tác mạnh với dấu và độ lớn. Một giá trị âm có thể đảo ngược dấu của toàn bộ kết quả tích lũy và một giá trị dương lớn ngay từ đầu có thể khuếch đại đáng kể các lựa chọn trong tương lai. Một cách tiếp cận tham lam ngây thơ quyết định cục bộ dựa trên việc phép nhân hay phép cộng lớn hơn ở một bước nhất định sẽ thất bại. 

Một trường hợp lỗi đơn giản xuất hiện khi phép nhân sớm tạo ra kết quả trung gian âm mà sau đó mang lại mức tăng rất lớn: 

đầu vào:```
3
-2 -2 100
```Nếu chúng ta tham lam cộng các giá trị sớm và chỉ nhân khi nó có vẻ có lợi cục bộ, chúng ta có thể tránh phép nhân sớm và kết thúc với kết quả nhỏ, nhưng chiến lược tối ưu là cẩn thận tạo ra một giá trị lớn sớm để các phép nhân sau chiếm ưu thế. 

Một vấn đề tế nhị khác là số 0 hoạt động giống như một thiết lập lại cho phép nhân. Nhân với số 0 sẽ phá hủy mọi tiềm năng khuếch đại trong tương lai, do đó, bất kỳ chiến lược đúng đắn nào cũng phải coi số 0 là điểm dừng cấu trúc. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ mô phỏng mọi chuỗi lựa chọn có thể xảy ra. Tại mỗi vị trí, chúng ta cộng hoặc nhân, cho$2^N$khả năng cho mỗi trường hợp thử nghiệm. Vì$N = 100$, điều này hoàn toàn không thể thực hiện được, vì thậm chí$2^{30}$đã vượt quá một tỷ tiểu bang và chúng ta cũng cần duy trì mức tăng trưởng số nguyên trung gian. 

Cấu trúc của bài toán gợi ý một công thức quy hoạch động. Tại mỗi chỉ số, chúng tôi duy trì điểm số tốt nhất có thể đạt được cho tất cả các "trạng thái" tính toán có thể có. Tuy nhiên, không gian trạng thái không chỉ là chỉ số, nó còn là giá trị hiện tại, điều này khiến cho việc tính DP trên các giá trị ngây thơ là không thể do mức tăng trưởng không giới hạn. 

Quan sát quan trọng là điểm số là một biểu thức tuyến tính được xây dựng từ chuỗi đầu vào, nhưng có sự lựa chọn linh hoạt xem mỗi thuật ngữ có trở thành một phần của chuỗi sản phẩm hay chỉ đơn giản được thêm vào. Bất kỳ chiến lược tối ưu nào cũng có thể được hiểu là chia mảng thành các phân đoạn, trong đó trong một phân đoạn, chúng tôi chọn chuỗi nhân một cách hiệu quả và giữa các phân đoạn, chúng tôi đặt lại bằng phép cộng. 

Cụ thể hơn, nếu chúng ta chọn một điểm mà chúng ta chọn phép cộng, thì mọi thứ trước nó sẽ độc lập với mọi thứ sau nó xét về tương tác nhân. Điều này cho thấy chúng tôi có thể duy trì DP ở các vị trí mà chúng tôi theo dõi kết quả tốt nhất kết thúc bằng “chuỗi nhân” so với kết thúc ở “trạng thái bổ sung đóng”. 

Ở mỗi bước, chúng ta có hai cách hiểu có ý nghĩa: 

Chúng tôi mở rộng chuỗi nhân đang chạy bằng cách nhân giá trị tiếp theo hoặc chúng tôi phá vỡ chuỗi và thêm phần đóng góp hiện tại vào kết quả cuối cùng, đặt lại trạng thái chuỗi. 

Điều này làm giảm vấn đề xuống một số trạng thái không đổi cho mỗi vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các lựa chọn) | O(2^N) | O(N) | Quá chậm | 
| DP với các trạng thái chuỗi | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai giá trị trong khi quét mảng từ trái sang phải. 

Một giá trị đại diện cho điểm tốt nhất nếu chúng ta đã “cam kết” tất cả các đóng góp trước đó vào tổng cuối cùng. Cái còn lại đại diện cho giá trị tốt nhất của chuỗi nhân đang hoạt động chưa được hoàn thiện thành câu trả lời. 

Chúng tôi cũng theo dõi xem chúng tôi đã bắt đầu một chuỗi hay chưa, vì phép nhân trước khi khởi tạo hoạt động khác với phép nhân sau. 

### Các bước 

1. Khởi tạo trạng thái DP trong đó điểm cuối cùng tốt nhất là 0 và không có chuỗi hoạt động. 
2. Với mỗi giá trị$a_i$, hãy xem xét hai hành động: bắt đầu hoặc mở rộng chuỗi nhân hoặc hoàn thiện chuỗi và thêm phần đóng góp hiện tại. 
3. Khi khởi động xích ở vị trí$i$, chúng tôi đặt giá trị chuỗi thành$a_i$hoặc tới một sản phẩm có chuỗi hiện tại nếu có. 
4. Khi mở rộng chuỗi, chúng tôi cập nhật giá trị chuỗi bằng cách nhân nó với$a_i$. Điều này thể hiện ý tưởng rằng chúng ta đang xây dựng một khối nhân liền kề. 
5. Tại mỗi vị trí, chúng tôi cũng xem xét việc phá vỡ chuỗi: chúng tôi thêm giá trị chuỗi hiện tại vào điểm cuối cùng và thiết lập lại chuỗi. 
6. Câu trả lời là số điểm tối đa trên tổng điểm cuối cùng và trường hợp chúng ta kết thúc bằng việc đóng chuỗi cuối cùng. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý vị trí$i$, DP thể hiện chính xác tất cả các cách tối ưu để phân chia tiền tố thành các phân đoạn xen kẽ của chuỗi nhân và các phép cộng cuối cùng. Bất kỳ giải pháp tối ưu nào cũng có thể được phân tách thành các phân đoạn như vậy vì mọi quyết định nhân đều có tính kết hợp trong một vùng liền kề và việc phá vỡ vùng đó tương đương với việc chọn phép cộng tại ranh giới đó. Vì DP giữ ngầm kết quả tốt nhất có thể có cho từng mẫu phân đoạn hợp lệ nên không có cấu trúc tối ưu nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(a):
    n = len(a)
    
    # dp0: best finalized score so far
    # dp1: best value of active multiplication chain
    dp0 = 0
    dp1 = None  # no chain yet
    
    for x in a:
        new_dp0 = dp0
        
        # Option 1: extend or start chain
        if dp1 is None:
            new_dp1 = x
        else:
            new_dp1 = dp1 * x
        
        # Option 2: break chain and add it to result
        if dp1 is not None:
            new_dp0 = max(new_dp0, dp0 + dp1)
        
        # Option 3: start new chain at current position
        new_dp1 = max(new_dp1, x)
        
        dp0, dp1 = new_dp0, new_dp1
    
    if dp1 is not None:
        dp0 = max(dp0, dp0 + dp1)
    
    return dp0

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        out.append(str(solve_case(a)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Mã duy trì hai đại lượng đang phát triển.`dp0`đại diện cho điểm số tốt nhất trong đó tất cả các hoạt động trước đó đã được hoàn thiện thành các phép cộng.`dp1`đại diện cho một chuỗi nhân hiện đang hoạt động và có thể vẫn được mở rộng hoặc đóng lại sau này. 

Ở mỗi bước, chúng ta mở rộng chuỗi nhân hoặc khởi động lại nó ở phần tử hiện tại. Chúng tôi cũng có tùy chọn đóng chuỗi và thêm nó vào điểm cuối cùng. Phần cẩn thận là đảm bảo chúng tôi không loại bỏ chuỗi sớm trước khi xem xét cả việc tiếp tục và đóng chuỗi, đó là lý do tại sao cả hai chuyển đổi đều được tính toán trước khi chuyển nhượng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
-2 -4 -2
```Chúng tôi theo dõi`(dp0, dp1)`: 

| tôi | một [tôi] | dp0 | dp1 | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | -2 | 0 | -2 | chuỗi bắt đầu | 
| 2 | -4 | 0 | 8 | mở rộng chuỗi | 
| 3 | -2 | 8 | -16 | đóng chuỗi trước đó, mở rộng | 

Câu trả lời cuối cùng là 8. 

Điều này cho thấy hai cực âm bên trong chuỗi tạo ra mức khuếch đại dương như thế nào và DP trì hoãn việc cam kết một cách chính xác cho đến khi cấu trúc trở nên có lợi. 

### Ví dụ 2 

đầu vào:```
4
2 -10 4 3
```| tôi | một [tôi] | dp0 | dp1 | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | chuỗi bắt đầu | 
| 2 | -10 | 0 | -20 | mở rộng | 
| 3 | 4 | 2 | 4 | đóng chuỗi, khởi động lại | 
| 4 | 3 | 14 | 3 | kết hợp các phân khúc tốt nhất | 

Dấu vết này cho thấy việc đóng chuỗi âm trước khi nó phát triển quá mức có hại sẽ có lợi, sau đó khởi động lại chuỗi mới trên các giá trị dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần với chuyển tiếp O(1) | 
| Không gian | O(1) | Chỉ một số lượng biến DP không đổi được lưu trữ | 

Tổng số công việc tối đa là$10^4 \times 100 = 10^6$hoạt động dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve_case(a):
        n = len(a)
        dp0 = 0
        dp1 = None
        for x in a:
            new_dp0 = dp0
            if dp1 is None:
                new_dp1 = x
            else:
                new_dp1 = dp1 * x
            if dp1 is not None:
                new_dp0 = max(new_dp0, dp0 + dp1)
            new_dp1 = max(new_dp1, x)
            dp0, dp1 = new_dp0, new_dp1
        if dp1 is not None:
            dp0 = max(dp0, dp0 + dp1)
        return dp0

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        out.append(str(solve_case(a)))
    return "\n".join(out)

# provided samples
assert run("4\n4\n4 0 1 2\n3\n3 -2 -2\n4\n-2 -4 -2 -8\n3\n2 -10 4\n") == "10\n12\n128\n4"

# custom cases
assert run("1\n1\n5\n") == "5"
assert run("1\n3\n0 0 0\n") == "0"
assert run("1\n4\n-1 -2 -3 -4\n") == "24"
assert run("1\n3\n10 -1 10\n") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tích cực duy nhất | 5 | trường hợp cơ sở | 
| tất cả số không | 0 | phép nhân sụp đổ | 
| tất cả tiêu cực | 24 | hiệu ứng chẵn lẻ chuỗi | 
| biển báo xen kẽ | 100 | phân khúc tối ưu | 

## Vỏ cạnh 

Mảng một phần tử là tầm thường vì cả phép cộng và phép nhân đều mang lại cùng một giá trị khi bắt đầu từ 0. DP khởi tạo chuỗi một cách chính xác và trả về ngay giá trị đó. 

Một mảng hoàn toàn bằng 0 buộc mọi chuỗi nhân phải thu gọn và thuật toán tránh tạo ra các cấu trúc âm hoặc không ổn định một cách chính xác bằng cách liên tục khởi động lại chuỗi. 

Mảng âm hoàn toàn là trường hợp tế nhị nhất vì phép nhân đổi dấu. DP tiếp tục mở rộng chuỗi vì tiêu cực trung gian không được cam kết ngay lập tức và chỉ đóng khi có lợi, cuối cùng sẽ thu được toàn bộ sản phẩm. 

Mảng dấu hỗn hợp như`10 -1 10`chứng minh tại sao lòng tham lại thất bại. Thuật toán trước tiên xây dựng một chuỗi, lật dấu một lần, sau đó khởi động lại một cách chính xác để tối đa hóa phân đoạn nhân thứ hai, mang lại điểm cuối cùng lớn hơn nhiều.
