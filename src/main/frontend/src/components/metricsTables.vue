<script>

    import { uuid } from './common.vue';

    export const metricsTable = {
        template: '<Table :id="id" :data="data" :columns="columns" height="600"></Table>',
        props: {
            id: {
                default: () => `chart-id-${uuid()}`
            },
            data: {
                default: []
            }
        },
        data() {
            return {
                columns: [{
                    key: 'urlPattern',
                    title: 'url',
                    sortable: true
                }, {
                    key: 'cnt',
                    title: '总数',
                    width: 100,
                    sortable: true
                }, {
                    key: 'errorCnt',
                    title: '错误数',
                    width: 100,
                    sortable: true
                }, {
                    key: 'max',
                    title: 'max (ms)',
                    width: 110,
                    sortable: true
                }, {
                    key: 'min',
                    title: 'min (ms)',
                    width: 110,
                    sortable: true
                }, {
                    key: 'avg',
                    title: 'avg (ms)',
                    width: 110,
                    sortable: true
                }, {
                    key: 'action',
                    title: '操作',
                    width: 110,
                    render: (h, params) => {
                        return h('div', [
                            h('Button', {
                                props: {
                                    type: 'text',
                                    size: 'small',
                                },
                                style: {
                                    'margin-left': '-10px'
                                },
                                on: {
                                    click: () => {
                                        this.$emit('on-show-request', params);
                                    }
                                }
                            }, '请求分布'),
                            h('Button', {
                                props: {
                                    type: 'text',
                                    size: 'small',
                                },
                                style: {
                                    'margin-left': '-10px'
                                },
                                on: {
                                    click: () => {
                                        this.$emit('on-show-distribution', params);
                                    }
                                }
                            }, '耗时分布')
                        ]);
                    }
                }]
            }
        }
    };

    export const reportTable = {
        template: '<Table :id="id" :data="data" :columns="columns" height="600"></Table>',
        props: {
            id: {
                default: () => `chart-id-${uuid()}`
            },
            data: {
                default: []
            }
        },
        data() {
            return {
                columns: [{
                    key: 'url',
                    title: 'url',
                    sortable: true
                }, {
                    key: 'cnt',
                    title: '总数',
                    width: 140,
                    sortable: true
                }, {
                    key: 'errorRatio',
                    title: '错误比例 (%)',
                    width: 140,
                    sortable: true,
                    render: (h, params) => {
                        return (params.row.errorRatio * 100).toFixed(2) + '%';
                    }
                }, {
                    key: 'slowRatio',
                    title: '慢请求比例 (%)',
                    width: 140,
                    sortable: true,
                    render: (h, params) => {
                        return (params.row.slowRatio * 100).toFixed(2) + '%';
                    }
                }, {
                    key: 'status',
                    title: '状态',
                    width: 140,
                    render: (h, params) => {
                        if (params.row.status == 'SLOW_WARNING') {
                            return h('div', [
                                h('Tag', {
                                    props: {
                                        color: 'yellow',
                                        type: 'border'
                                    },
                                    style: {
                                        'margin-left': '-10px'
                                    }
                                }, '慢请求警告')
                            ]);
                        }
                        else if (params.row.status == 'ERROR_WARNING') {
                            return h('div', [
                                h('Tag', {
                                    props: {
                                        color: 'red',
                                        type: 'border'
                                    },
                                    style: {
                                        'margin-left': '-10px'
                                    }
                                }, '错误警告')
                            ]);
                        }
                    }
                }]
            }
        }
    };

</script>