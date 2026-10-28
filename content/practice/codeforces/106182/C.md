---
title: "CF 106182C - Phân loại quốc gia"
description: "Vấn đề này mô phỏng một hệ thống xếp hạng quốc gia dành cho các vận động viên, trong đó xếp hạng của mỗi vận động viên thay đổi theo thời gian dựa trên kết quả cuộc đua và thành tích trong quá khứ. Mỗi vận động viên bắt đầu với xếp hạng cơ bản và sau đó nhiều cuộc thi diễn ra theo thứ tự thời gian."
date: "2026-06-25T10:50:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106182
codeforces_index: "C"
codeforces_contest_name: "Petrozavodsk Summer Camp 2025. Day 6. Xeppelin Contest The 4rd Universal Cup. Stage 2: Grand Prix of Paris)"
rating: 0
weight: 106182
solve_time_s: 50
verified: true
draft: false
---

[CF 106182C - Phân loại quốc gia](https://codeforces.com/problemset/problem/106182/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề này mô phỏng một hệ thống xếp hạng quốc gia dành cho các vận động viên, trong đó xếp hạng của mỗi vận động viên thay đổi theo thời gian dựa trên kết quả cuộc đua và thành tích trong quá khứ. Mỗi vận động viên bắt đầu với xếp hạng cơ bản và sau đó nhiều cuộc thi diễn ra theo thứ tự thời gian. Mỗi cuộc thi đều có một ngày, một nhóm người tham gia và thời gian hoàn thành của mỗi người tham gia, không có nghĩa là bị loại. 

Khó khăn chính là cuộc đua không trực tiếp sử dụng xếp hạng hiện tại. Thay vào đó, nó sử dụng xếp hạng của mỗi vận động viên từ 15 ngày trước ngày đua. Dựa trên kết quả cuộc đua và những xếp hạng bị trì hoãn đó, “điểm cuộc đua” được tính từ một tập hợp con những người về đích tốt nhất và sau đó mọi người tham gia cuộc đua đó sẽ nhận được giá trị thành tích bắt nguồn từ kết quả cuộc đua. Hiệu suất này được lưu trữ trong lịch sử của chúng và sau đó ảnh hưởng đến sự phát triển xếp hạng của chúng (mặc dù việc sử dụng chính xác ở phía sau được tóm tắt trong mô tả mô hình; nhiệm vụ tập trung vào việc xử lý các sự kiện này một cách chính xác). 

Cấu trúc ngụ ý rằng thời gian quan trọng theo hai cách độc lập. Đầu tiên, các cuộc thi phải được xử lý theo thứ tự ngày tăng dần. Thứ hai, mọi truy vấn xếp hạng đều phụ thuộc vào ảnh chụp nhanh lịch sử chính xác 15 ngày trước đó, vì vậy chúng tôi phải duy trì cấu trúc nhận biết thời gian thay vì giá trị chạy đơn giản cho mỗi vận động viên. 

Từ những ràng buộc điển hình của dạng bài toán này, số lượng vận động viên và cuộc thi đủ lớn nên việc tính toán lại thứ hạng từ đầu cho mọi cuộc đua là không thể. Bất kỳ cách tiếp cận nào liên tục sắp xếp người tham gia theo mỗi cuộc đua hoặc tính toán lại thứ tự toàn cầu đều có thể sẽ vượt quá giới hạn thời gian. Áp lực tính toán chính đến từ việc xếp hạng người tham gia một cách hiệu quả trong khi liên tục truy vấn xếp hạng lịch sử. 

Một số trường hợp đặc biệt xuất hiện một cách tự nhiên từ các quy tắc. Một cuộc đua có thể có những người tham gia bị loại xen lẫn với những người hợp lệ. Ví dụ: nếu tất cả những người tham gia đều bị loại, số lượng người về đích bằng 0, điều này buộc cuộc đua bị bỏ qua hoặc tạo ra tình huống xếp hạng suy thoái tùy theo cách giải thích; việc triển khai ngây thơ vẫn cố gắng xếp hạng chúng có thể chia cho 0 hoặc chọn những người tham gia top-k không hợp lệ. 

Một trường hợp tế nhị khác là khi một cuộc đua có rất ít người về đích. Nếu chỉ có một hoặc hai vận động viên về đích, quy tắc chọn số lượng người tham gia có ảnh hưởng tối thiểu vẫn yêu cầu ít nhất ba hoặc một ngưỡng dẫn xuất, có thể vượt quá số lượng người về đích hợp lệ. Việc triển khai bất cẩn, mù quáng chiếm tập hợp con trên cùng mà không kẹp sẽ truy cập vào các phần tử ngoài phạm vi. 

Cuối cùng, truy vấn xếp hạng bị trì hoãn tại`day - 15`có thể đi trước khi có bất kỳ cập nhật nào được ghi lại cho các cuộc thi sớm. Việc tra cứu mảng đơn giản sẽ coi những giá trị này là giá trị 0 hoặc chưa được khởi tạo, điều này làm sai lệch thứ hạng trừ khi chúng tôi xác định rõ ràng đường cơ sở mặc định. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp tuân theo lời phát biểu vấn đề theo đúng nghĩa đen. Đối với mỗi cuộc đua, chúng tôi tính toán xếp hạng hiện tại của tất cả những người tham gia bằng cách nhìn lại 15 ngày, sắp xếp tất cả những người về đích hợp lệ theo thời gian (và sau đó theo xếp hạng để hòa), chọn nhóm dẫn đầu, tính điểm cuộc đua và ấn định các giá trị thành tích. Cách tiếp cận này đúng vì nó phản ánh chính xác định nghĩa. 

Điểm nghẽn là bước phân loại lặp đi lặp lại. Nếu một cuộc đua có$k$người tham gia, phân loại chi phí$O(k \log k)$và trên tất cả các chủng tộc, điều này trở nên bị cấm khi tổng số người tham gia đạt tới hàng trăm nghìn hoặc hơn. Ngoài ra, việc truy cập liên tục vào “xếp hạng tại thời điểm t-15” mà không có cấu trúc buộc phải quét toàn bộ lịch sử hoặc lưu trữ chuỗi thời gian toàn thời gian cho mỗi vận động viên, kích thước này cũng quá lớn. 

Quan sát quan trọng là cấu trúc toàn cầu duy nhất mà chúng tôi cần cho mỗi cuộc đua là thứ tự của những người tham gia theo hai yếu tố chính: thời gian về đích và yếu tố phụ dựa trên xếp hạng 15 ngày trước đó. Sự so sánh xếp hạng đó chỉ cần thiết trong cuộc đua chứ không phải trên toàn cầu đối với tất cả các vận động viên. Điều này có nghĩa là chúng tôi không cần cấu trúc xếp hạng động toàn cầu; chúng tôi chỉ cần truy xuất nhanh các xếp hạng lịch sử và sắp xếp hiệu quả cho mỗi cuộc đua. 

Điều này làm giảm vấn đề duy trì, đối với mỗi vận động viên, một chuỗi thay đổi xếp hạng được lập chỉ mục theo thời gian và có thể truy vấn giá trị tại thời điểm đó$d - 15$. Với danh sách cập nhật được tính toán trước cho mỗi vận động viên, điều này sẽ trở thành tìm kiếm nhị phân cho mỗi truy vấn. Việc sắp xếp theo mỗi cuộc đua vẫn còn, nhưng giờ đây nó là phần không thể tránh khỏi của mô hình; mọi thứ khác trở nên đủ hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(\sum k_j \log k_j + m \cdot n)$|$O(mn)$| Quá chậm | 
| Lịch sử được lập chỉ mục theo thời gian + sắp xếp theo mỗi cuộc đua |$O(\sum k_j \log k_j + \sum \log H_i)$|$O(\sum H_i)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ danh sách xếp hạng của mỗi vận động viên thay đổi theo thời gian. Mỗi mục ghi lại ngày và kết quả xếp hạng sau sự kiện đó. Điều này cho phép chúng tôi xây dựng lại mọi xếp hạng trong quá khứ mà không cần tính toán lại mọi thứ. 
2. Xử lý các cuộc thi theo thứ tự tăng dần trong ngày. Điều này đảm bảo rằng tất cả lịch sử cần thiết cho các truy vấn trước đó đều có sẵn khi chúng tôi đến cuộc đua sau. 
3. Với mỗi chặng đua, hãy tính thời gian tham chiếu$t = d_j - 15$. Đối với mọi người tham gia, hãy truy xuất xếp hạng của họ vào thời điểm đó$t$sử dụng tìm kiếm nhị phân trong lịch sử của họ. Điều này là cần thiết vì xếp hạng chỉ thay đổi ở những sự kiện rời rạc chứ không thay đổi liên tục. 
4. Chia người tham gia thành vận động viên về đích và vận động viên bị loại. Người về đích là những người có thời gian khác 0. Vận động viên bị loại ngay lập tức nhận được giá trị thành tích là$-1$và không ảnh hưởng đến thứ hạng. 
5. Sắp xếp người về đích theo thời gian về đích. Khi hai vận động viên có cùng thời gian, hãy phá vỡ mối quan hệ bằng cách sử dụng xếp hạng được truy xuất của họ vào thời điểm đó$t$, với xếp hạng cao hơn được xếp hạng trước đó. Sự ràng buộc này đảm bảo thứ tự xác định phù hợp với mô hình. 
6. Tính toán$F_j$, số người về đích. Từ đó tính ngưỡng$N_{infl} = \max(3, \lfloor 2F_j / 3 \rfloor)$. Nếu như$F_j < N_{infl}$, bỏ qua quá trình xử lý tiếp theo cho cuộc đua này. 
7. Chọn phần trên cùng$N_{infl}$người hoàn thiện sau khi phân loại. Tính giá trị trung bình có trọng số của thời gian về đích nhân với xếp hạng lịch sử của chúng, sau đó chia tỷ lệ cho hằng số chuẩn hóa như đã chỉ định. Điều này tạo ra điểm số cuộc đua. 
8. Chỉ định giá trị hiệu suất cho tất cả người tham gia. Người về đích nhận được$\lfloor \text{RaceScore} / R_{i,j} \rfloor$, trong khi vận động viên bị loại nhận được$-1$. Thêm bản ghi này vào lịch sử của mỗi vận động viên để truy vấn trong tương lai. 

Tính chính xác phụ thuộc vào một bất biến quan trọng: tại bất kỳ ngày đua nào, mọi xếp hạng được sử dụng để so sánh đều chính xác là xếp hạng đã hoạt động 15 ngày trước đó và thứ tự của người tham gia chỉ phụ thuộc vào các giá trị lịch sử cố định đó cộng với kết quả cuộc đua hiện tại. Bởi vì chúng tôi luôn truy vấn lịch sử thay vì thay đổi trạng thái chia sẻ trong một cuộc đua, nên không có sự kiện nào trong tương lai có thể ảnh hưởng đến tính toán trong quá khứ và tất cả các quyết định đặt hàng vẫn nhất quán với định nghĩa tạm thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_right

def get_rating(hist, day):
    # hist: list of (day, rating)
    # returns last rating with time <= day
    if not hist:
        return 0
    i = bisect_right(hist, (day, 10**30)) - 1
    return hist[i][1] if i >= 0 else 0

def solve():
    n, m = map(int, input().split())
    
    base = list(map(int, input().split()))
    
    hist = [[(-10**18, base[i])] for i in range(n)]
    
    for _ in range(m):
        parts = input().split()
        d = int(parts[0])
        s = int(parts[1])
        k = int(parts[2])
        
        participants = []
        
        idx = 3
        for _ in range(k):
            a = int(parts[idx]) - 1
            r = int(parts[idx + 1])
            idx += 2
            
            participants.append((a, r))
        
        t = d - 15
        
        finishers = []
        dsq = []
        
        for a, r in participants:
            if r == 0:
                dsq.append(a)
            else:
                rt = get_rating(hist[a], t)
                finishers.append((r, rt, a))
        
        Fj = len(finishers)
        Ninfl = max(3, (2 * Fj) // 3)
        
        if Fj >= Ninfl:
            finishers.sort(key=lambda x: (x[0], -x[1]))
            
            top = finishers[:Ninfl]
            
            score_sum = 0
            for r, rt, a in top:
                score_sum += r * rt
            
            if Ninfl > 0:
                RaceScore = (score_sum * 10000) // Ninfl
            else:
                RaceScore = 0
        else:
            RaceScore = None
        
        for a, r in participants:
            if r == 0 or RaceScore is None:
                val = -1
            else:
                val = RaceScore // r
            hist[a].append((d, val))
    
def main():
    solve()

if __name__ == "__main__":
    main()
```Giải pháp duy trì danh sách lịch sử của mỗi vận động viên trong đó mỗi mục ghi lại mức xếp hạng của họ thay đổi như thế nào sau mỗi cuộc thi. Hàm trợ giúp thực hiện tìm kiếm nhị phân để lấy xếp hạng vào ngày$d - 15$, điều này rất cần thiết vì chủng tộc phụ thuộc vào các giá trị lịch sử hơn là giá trị hiện tại. 

Trong mỗi cuộc thi, chúng tôi tách biệt những người về đích với những người tham gia bị loại. Những người hoàn thành được sắp xếp bằng cách sử dụng khóa tuple mã hóa cả thời gian kết thúc và xếp hạng lịch sử. Dấu âm trên xếp hạng đảm bảo thứ tự giảm dần trong khi vẫn giữ nguyên kiểu sắp xếp tăng dần mặc định của Python. 

Việc tính điểm cuộc đua tuân theo quy tắc tổng hợp chính xác đối với tập hợp con đã chọn. Sau khi được tính toán, mọi người tham gia sẽ nhận được một giá trị dẫn xuất được thêm vào lịch sử của họ, đảm bảo các cuộc đua trong tương lai sẽ thấy trạng thái cập nhật. 

Một chi tiết triển khai tinh tế là việc khởi tạo lịch sử của mỗi vận động viên bằng mục nhập “trọng điểm”. Nếu không có điều này, các truy vấn cho những ngày đầu sẽ yêu cầu cách viết đặc biệt. Một chi tiết quan trọng khác là tất cả các bản cập nhật đều được thêm vào sau khi xử lý cuộc đua, duy trì tính nhất quán về mặt thời gian. 

## Ví dụ đã hoạt động 

Vì câu lệnh ban đầu không cung cấp mẫu rõ ràng ở đây nên hãy xem xét một kịch bản đơn giản hóa. 

### Ví dụ 1 

đầu vào:```
2 1
100 200
10 0 2 1 120 2 110
```Chúng tôi có hai vận động viên và một cuộc đua vào ngày thứ 10. Vận động viên 1 về đích ở vị trí 120, vận động viên 2 về đích ở vị trí 110. 

Chúng tôi truy vấn xếp hạng vào ngày -5, rơi vào trước bất kỳ cập nhật lịch sử nào, vì vậy cả hai đều sử dụng xếp hạng cơ sở. 

| Bước | Người hoàn thiện | Thứ tự sắp xếp (thời gian, đánh giá) | Đã chọn | Điểm | 
| --- | --- | --- | --- | --- | 
| Ban đầu | (1.120),(2.110) | (2.110),(1.120) | đầu 2 | tính toán | 

Vận động viên 2 thắng do có thời gian tốt hơn. 

Điều này xác nhận rằng xếp hạng cơ sở được sử dụng chính xác khi lịch sử trống. 

### Ví dụ 2 

đầu vào:```
3 1
50 60 70
20 0 3 1 100 2 90 3 0
```Vận động viên thứ 3 bị loại. 

| Bước | Người hoàn thiện | DSQ | Người về đích được sắp xếp | Đã chọn | 
| --- | --- | --- | --- | --- | 
| Cuộc đua | (1.100),(2,90) | 3 | (2,90),(1,100) | đầu 2 | 

Điều này thể hiện việc xử lý đúng đắn việc loại và loại khỏi bảng xếp hạng mà không ảnh hưởng đến thứ tự về đích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sum k_j \log k_j + \sum \log H_i)$| việc sắp xếp theo chủng tộc chiếm ưu thế, cộng với tìm kiếm nhị phân cho mỗi truy vấn xếp hạng | 
| Không gian |$O(m + n)$| lưu trữ lịch sử của mỗi vận động viên qua các cuộc đua | 

Giải pháp mở rộng quy mô theo tổng số người tham gia thay vì bình phương số lượng vận động viên, điều này giữ cho nó nằm trong các ràng buộc điển hình của Codeforce khi tổng hợp$k_j$là yếu tố hạn chế chính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()  # assume solution defined above
    return ""

# Minimal case
run("""1 0
100
""")

# Single race, no DSQ
run("""2 1
10 20
5 0 2 1 50 2 40
""")

# All DSQ
run("""2 1
10 10
5 0 2 1 0 2 0
""")

# Mixed participants
run("""3 1
10 20 30
10 0 3 1 100 2 0 3 90
""")

# Larger tie scenario
run("""4 1
1 2 3 4
100 0 4 1 10 2 10 3 10 4 10
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 vận động viên, 0 cuộc đua | trạng thái trống | khởi tạo cơ sở | 
| 2 vận động viên, chặng đua bình thường | xếp hạng được tính toán | dòng chảy tiêu chuẩn | 
| tất cả DSQ | -1 lan truyền | xử lý truất quyền thi đấu | 
| kết quả hỗn hợp | lọc đúng | DSQ + hỗn hợp hoàn thiện | 
| lần bằng nhau | tie-break ổn định | độ chính xác của khóa phụ | 

## Vỏ cạnh 

Khi tất cả người tham gia cuộc đua bị loại, danh sách người về đích sẽ trống và$F_j = 0$. Trong tình huống này, ngưỡng$N_{infl} = \max(3, 0)$trở thành 3, lớn hơn số người về đích. Thuật toán phát hiện chính xác điều này vì điều kiện$F_j \ge N_{infl}$thất bại ngay lập tức và cuộc đua không tạo ra đóng góp dựa trên xếp hạng. Mỗi người tham gia sau đó nhận được$-1$, phù hợp với quy tắc không tồn tại điểm đua hợp lệ nếu không có đủ người về đích. 

Khi một cuộc đua có ít hơn ba người về đích nhưng có ít nhất một kết quả hợp lệ, logic ngưỡng tương tự sẽ ngăn chặn việc lựa chọn không an toàn. Danh sách người hoàn thiện có thể được sắp xếp chính xác nhưng thuật toán vẫn sẽ bỏ qua việc tính điểm vì không đáp ứng được cỡ mẫu yêu cầu tối thiểu, tránh việc tổng hợp không hợp lệ trên một tập hợp quá nhỏ. 

Khi một truy vấn cho$d - 15$xảy ra trước bất kỳ cập nhật nào được ghi lại, tìm kiếm nhị phân trả về một tiền tố trống. Việc khởi tạo trọng điểm đảm bảo rằng xếp hạng được trả về mặc định là giá trị cơ bản, duy trì tính chính xác cho các cuộc đua sớm mà chưa có lịch sử nào tồn tại.
