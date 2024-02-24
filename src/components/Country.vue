<template>
        <div class="col-md-4 bg-secondary mx-auto">
            <div class="card ">
                <slot name="icon"></slot>
                <div class="card-body">
                    <slot name="heading text-black"></slot>
                     <slot></slot>
                </div>
            </div>
        </div>
</template>