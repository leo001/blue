# test-files
//最小化时间继续走的timer
interface IObserver {
    start();
    stop();
    onTimer();
    ts: number;
}


class GTimer {
    // public timer: egret.Timer;
    public observers: Array<IObserver>
    private tid: number;

    constructor() {

        this.tid = undefined;
        // this.timer = new egret.Timer(1000);
        // this.timer.addEventListener(egret.TimerEvent.TIMER, this.onTimer, this);

        this.observers = [];
    }

    public register(observer: IObserver): void {
        if (-1 == this.observers.indexOf(observer)) {
            this.observers.push(observer);
        }
        // if (!this.timer.running && this.hasElement()) {
        //     this.timer.start();
        // }
        if (undefined == this.tid && this.hasElement()) {
            this.tid = setTimeout(onTimer, 1000);
        }
        var self = this;
        function onTimer(): void {
            egret.log(1111);
            let e: IObserver;
            for (let i = self.observers.length - 1; i >= 0; i--) {
                e = self.observers[i];
                if (e) {
                    e.onTimer();
                } else {
                    egret.log('[LOGIC_ERROR]:onTimer()');
                }
            }
            self.tid = setTimeout(onTimer, 1000);
        }
    }
    public unRegister(observer: IObserver): void {
        let idx = this.observers.indexOf(observer);
        if (-1 != idx) {
            this.observers.splice(idx, 1);
        }

        // if (this.timer.running && !this.hasElement()) {
        //     this.timer.stop();
        // }
        if (undefined != this.tid && !this.hasElement()) {
            clearTimeout(this.tid);
            this.tid = undefined;
        }
    }

    public hasElement(): boolean {
        return this.observers.length > 0;
    }
}
